# DRAFT - ojuny-console-installer

Status: draft scope. Nothing is built. This file describes what the repository
is supposed to contain, not what it contains today.

## Purpose

The install media for Ojuny Console. It turns a generic x86_64 machine into a
living-room console with the minimum number of prompts: boot, choose a disk,
create the console account, install, reboot into Steam Big Picture Mode.

It installs the Console template by default and keeps the generic installer
behavior, so another Ojuny template can still be installed from the same media.

## What the repository will contain

```text
ojuny-console-installer/
  flake.nix                  ISO and image outputs
  nixos/                     live profiles for each variant
  installer/                 console install flow
    disk/                    target selection and layout
    account/                 console account and auto-login
    first-boot/              Steam and controller setup
    management/              optional nix-web-gui setup
  docs/                      install guide, first boot, recovery
  tests/                     VM and hardware checklists
```

### Install media

- Attended online ISO.
- Attended offline ISO with the Console closure.
- Attended discovery ISO using local Ojuny cache peers.
- Unattended ISO with a declarative answer file.
- A raw disk image for dedicated console hardware, if the persistent-image
  path is reused.

### Install flow

1. Boot the media. The console template is selected by default.
2. Detect the target disk and confirm the destructive install.
3. Optionally encrypt the disk. The console account password is still required
   for recovery.
4. Create the console account and enable auto-login.
5. Choose the display mode: TV, monitor or automatic.
6. Install, reboot and start Steam Big Picture.
7. Pair a controller and sign in to Steam on first boot.
8. Optionally enable `nix-web-gui` and print its URL and access token.

### First boot

- Steam starts in Big Picture Mode without a desktop.
- A controller pairing screen appears if no controller is detected.
- A short console setup page covers display, audio and network.
- The desktop target is explained, not forced.
- The management URL is shown only when `nix-web-gui` is enabled.

### Variants

- Online, offline and discovery images follow the same pattern as the other
  Ojuny installers.
- The unattended image installs the console preset without interaction.
- A recovery image or recovery entry restores the console to the installed
  state without reinstalling.

### Documentation

- The console install journey and the minimum hardware.
- Disk, encryption and auto-login tradeoffs.
- Offline and discovery image requirements.
- First-boot controller and Steam setup.
- How to reach the desktop and how to enable remote management.
- Recovery from a failed install or a broken session.

### Tests

- Each ISO boots in QEMU.
- An install reaches Steam Big Picture with auto-login.
- The desktop target starts after install.
- An offline install completes without network access.
- A non-Console Ojuny template can be installed from the same media.
- The encrypted path boots and the account can unlock it.
- `nix-web-gui` is unreachable unless enabled.

## Limits and non-goals

- `x86_64-linux` only.
- The unattended path wipes the selected disk.
- No Wi-Fi configuration in the unattended path.
- No Steam account provisioning and no game installation automation.
- No Secure Boot enrollment.
- No console hardware certification and no handheld support in the first
  scope.
- No guarantee that a given game works under Proton.

## Open questions

- Does the installer need a controller-driven UI, or is a keyboard acceptable
  on first install?
- Is the console account password required, or is auto-login without a password
  allowed when disk encryption is enabled?
- Should `nix-web-gui` be offered on the first-boot screen or only after the
  desktop is reached?
- Is a recovery partition or a second boot entry required?
- Does the raw image path belong here or in the core installer?

## Sources

- `ojuny-installer/README.md` and `docs/persistent-image.md`,
  `docs/offline-install.md`, `docs/unattended-install.md`.
- `nixos-configs/notes/Ojuny Deployment Methods.md`.
- `ojuny-template/docs/03-host-setup.md`.
