# iSH-X Architecture

## 1. Product boundary
iSH-X runs a Linux AArch64 userspace inside an iOS application. It is not a complete Linux VM with an independent Linux kernel. Linux compatibility is provided through syscall translation, process/memory/filesystem emulation, and CPU emulation/JIT.

The guest root user is simulated/fake root. It must never be represented as iOS/XNU root.

## 2. CPU execution
Host: iOS ARM64. Guest: Linux AArch64.

Execution:
Linux AArch64 -> Linux syscall/process model -> AArch64 CPU state and memory -> Gadget JIT or optional StikDebug/StikJIT path.

The gadget JIT is always the functional fallback. JIT states are DISABLED, DETECTING, STIKDEBUG_READY, GADGET_READY, and ERROR. Startup must never depend on StikDebug. Opening a StikDebug URL is not proof that JIT is active; readiness must be verified.

## 3. Root filesystem
Alpine Linux AArch64 is the primary guest distribution. The rootfs is an external/importable artifact rather than a mandatory IPA payload.

The guest must support normal apk operation. Expected initial state: uid=0(root), uname -m -> aarch64, apk update, and installation of Rust, Python, Git and other tools through apk.

## 4. iOS capability bridge
Linux accesses iOS capabilities through public APIs:

Linux command -> iosctl/bridge device -> NativeBridge -> public iOS framework + permission -> result/file/event.

Initial domains: Camera, Microphone/audio, Location, Bluetooth, Motion, Photos, Contacts, Calendar, Reminders, Notifications, Clipboard, Battery/device information, and local-network services.

Every request is permission-gated and guest-controlled arguments and paths are validated before crossing the native boundary.

## 5. CLI contract
Initial commands include:
- iosctl camera photo /home/user/photo.jpg
- iosctl microphone record --duration 10 /home/user/audio.wav
- iosctl location get
- iosctl location watch
- iosctl bluetooth scan
- iosctl motion accelerometer
- iosctl photos save /tmp/photo.jpg
- iosctl contacts list
- iosctl calendar list
- iosctl reminders list
- iosctl notify --title "Test" --body "Hello"
- iosctl clipboard get/set
- iosctl battery
- iosctl device

The CLI is a Linux guest program; it does not contain private iOS API calls.

## 6. Binary data
Photos, recordings and other large payloads should use guest/shared files whenever practical. IPC carries metadata, status/errors, small structured results, or handles to larger data.

## 7. Security boundary
Never expose camera, Bluetooth or other controllers as raw Linux devices. Never expose private iOS frameworks, XNU interfaces, arbitrary host paths, or unrestricted native function invocation.

Every bridge operation requires command validation, argument validation, sandbox/path validation, iOS authorization, bounded resource use, and structured error propagation.

## 8. IPA composition
The IPA contains the runtime, UI, NativeBridge and only necessary small native helpers. Alpine rootfs and development packages are external artifacts. Rust, Python, Git and similar guest tools are not bundled into the IPA.

## 9. Non-goals
- iOS/XNU root access
- private APIs
- arbitrary kernel modules
- direct USB/GPU/controller passthrough
- unrestricted host filesystem access
- a complete Linux kernel
