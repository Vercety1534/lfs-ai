# lfs-ai

**LFS-AI: Linux From Scratch automated installer**

## Purpose

This project exists to reduce the time and friction involved in getting a working LFS base system so you can move on to building a more useful system with BLFS.

It also includes a few carefully selected BLFS packages and dependencies to ease common post-install pain points, and installs Limine for EFI systems instead of GRUB.

It is not intended to replace understanding the LFS process. The LFS and BLFS books should still be treated as the primary references.

## What it does

- automates the major stages of an LFS install
- supports full installs or stage-by-stage execution
- uses numbered package scripts for ordered builds
- separates toolchain, chroot, LFS, and final system setup
- keeps logs to make troubleshooting easier
- uses `settings.conf` to control install behavior
- installs Limine for EFI systems instead of GRUB
- includes selected BLFS packages such as `wget`, `make-ca`, and required dependencies to reduce post-install setup work

## Requirements

- UEFI/EFI system
- Review `settings.conf` before install
- Understanding of the LFS build process

See `DEPS.md` for host system dependencies.

## Project Structure

```text
.
├── chroot/             # Temporary tool installs used during early chroot setup
├── cross-toolchain/    # Cross-toolchain build scripts
├── lfs/                # Main numbered LFS package scripts
├── lists/              # Package lists, configs, and download references
├── scripts/            # Wrapper/helper scripts for each stage
├── install             # LFS-AI: Linux From Scratch automated installer
├── package.template    # Template for package scripts
└── settings.conf       # Main user-editable configuration
```

## Usage

Run the installer with:

`./install`

**Basic Commands**

```text
./install -A    :  ALL   :   Complete install including drive partitioning
./install -V    :  DEPS  :   Verify dependencies on host system
./install -h    :  Help  :   Display help message
```

**Phased Commands**

```text
./install -s    : Step 1 :   Edit settings.conf (IMPORTANT - Failure to do so could destroy your system)
./install -p    : Step 2 :   Prepare the host system and partition the drive
./install -d    : Step 3 :   Download the required source packages
./install -t    : Step 4 :   Install the toolchain
./install -c    : Step 5 :   Prepare the chroot environment
./install -l #  : Step 6 :   Install LFS [Phases - 1|2|3|4|5|all]
./install -f    : Step 7 :   Install kernel, config files, and clean up build system
```

## Configuration

Before running anything, review and edit:

`settings.conf`

This file controls important install behavior such as:
- target disk
- EFI size
- swap size
- mount location
- script location
- passwords
- other system-specific settings

Incorrect settings can destroy data or overwrite the wrong system.

## Logging

Logs are written under:

`$LFS/logs/lfs/`

These logs are intended to make it easier to identify which stage or package failed.

## Warning

This project currently supports **UEFI/EFI systems only** and installs **Limine** as the bootloader.

This project can make destructive changes to a system, including:
- repartitioning disks
- formatting filesystems
- installing bootloaders
- modifying system configuration
- overwriting important data

**Use at your own risk.**

Always review `settings.conf` and test in a virtual machine before using real hardware.

## License

This project is licensed under the MIT License.
