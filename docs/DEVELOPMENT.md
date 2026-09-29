# iSH-X Development Contract

## Golden rules
1. Functionality before optional acceleration.
2. Gadget JIT remains the baseline.
3. StikDebug is optional.
4. ARM64 is the only guest architecture.
5. Alpine AArch64 is the primary rootfs.
6. Large development packages stay outside the IPA.
7. iOS capabilities use public APIs and explicit permissions.
8. Guest UID 0 is not iOS root.
9. No private API or direct controller access.
10. Every major change has a reproducible test.

## Development order
1. Import and pin the iSH-AOK baseline.
2. Remove/disable non-ARM64 product targets.
3. Establish clean ARM64 CI.
4. Establish external Alpine rootfs import/boot.
5. Lock syscall/process/filesystem regressions.
6. Make gadget JIT the mandatory runtime baseline.
7. Add JIT coordinator and StikDebug detection.
8. Add NativeBridge and iosctl.
9. Add location/motion.
10. Add camera/microphone/photos.
11. Add Bluetooth/network services.
12. Add Contacts/Calendar/Reminders/Notifications.
13. Preserve File Provider and Shortcuts where compatible.
14. Optimize, benchmark and harden.

## Foundation definition of done
A device build must eventually demonstrate:
- iSH-X launches.
- Alpine AArch64 boots.
- id reports guest root.
- uname -m reports aarch64.
- apk update succeeds.
- guest networking works.
- gadget JIT works without StikDebug.
- StikDebug detection never blocks startup.
- iosctl reaches NativeBridge.
- denied iOS permissions produce deterministic guest-visible errors.
