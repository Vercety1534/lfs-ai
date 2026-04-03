# Host Dependencies

LFS-AI requires a properly prepared Linux host system before installation.

These dependencies are checked with:

```text
./install -v
```

If dependency verification fails, install the missing packages on the host system before continuing. You should also review [`README.md`](README.md) and [`WORKFLOW.md`](WORKFLOW.md) before starting.

## Required Host Tools

The host system should have the standard tools expected for an LFS build environment, including:

`Coreutils`         >= 8.25

`Acl`               >= 2.2.49

`Bash`              >= 3.2

`Binutils`          >= 2.13.1

`Bison`             >= 2.7

`Diffutils`         >= 2.8.1

`Dosfstools`        >= 3.0.28

`E2fsprogs`         >= 1.45

`Findutils`         >= 4.2.31

`Gawk`              >= 4.0.1

`GCC`               >= 7.0

`GCC (C++)`         >= 7.0

`Grep`              >= 2.5.1a

`Gzip`              >= 1.3.12

`M4`                >= 1.4.10

`Make`              >= 4.0

`nvme-cli`          >= 1.9.0 (only required for systems with NVMe drives)

`Patch`             >= 2.5.4

`Perl`              >= 5.8.8

`Python`            >= 3.6

`Sed`               >= 4.1.5

`Shadow`            >= 4.8

`Tar`               >= 1.22

`Texinfo`           >= 5.0

`Util-linux`        >= 2.26

`Wget`              >= 1.16.0

`Xz`                >= 5.0.0


Minimum kernel version:

`Kernel`            >= 5.4


Required kernel option:

`CONFIG_UNIX98_PTYS=y`


## Required Host Headers

The host system must also provide:

- Linux kernel headers
- gettext headers

These are expected to provide at least:

- `/usr/include/linux/perf_event.h`
- `/usr/include/libintl.h`


## Compatibility Notes

LFS-AI is tested primarily on GNU-based Linux hosts.

Some hosts may provide alternate implementations of common tools, such as:

- BusyBox
- uutils
- non-Bash `/bin/sh`
- non-GNU `awk`
- missing `yacc` compatibility link

These may still work, but `./install -v` may report compatibility warnings even when the host is otherwise usable.


## Preferred Command Links

For best compatibility, the host should also provide these common command mappings:

`awk`              = `gawk`

`yacc`             = `bison`

`sh`               = `bash`


## Required Host Capabilities

The host system should also support:

- `chroot`
- root access
- mounting virtual kernel filesystems such as `/dev`, `/proc`, `/sys`, and `/run`
- UNIX 98 PTY support
- `/dev/ptmx`
- internet access for downloading source packages, unless all sources are already present locally

## Recommended Host Environment

For best results, the host system should:

- be a modern Linux distribution
- use a GNU-based userland where possible
- use UEFI/EFI if you plan to follow the full automated install flow
- have enough disk space for sources, build files, logs, and the target LFS system
- have enough RAM and swap for large packages such as GCC and Python
- ideally be tested in a virtual machine before being used on real hardware

## Important Notes

- LFS-AI is designed for UEFI/EFI systems only
- The automated install flow uses Limine instead of GRUB
- Networking setup only includes ethernet; follow the BLFS book for wireless networking
- Some BLFS packages are included during the build process to reduce post-install setup work
- Incorrect [`settings.conf`](settings.conf) values can destroy data or overwrite the wrong disk

## Before You Start

Before running the installer, make sure you have:

- reviewed and edited [`settings.conf`](settings.conf)
- verified the target disk is correct
- confirmed you are willing to erase or modify the selected disk
- checked that required host dependencies are installed
- confirmed the system is booted in UEFI mode

