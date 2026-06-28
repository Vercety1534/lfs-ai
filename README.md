# LFS-AI

LFS-AI is a **Linux From Scratch Automated Installer** designed to build and install an LFS system through a guided, script-driven workflow.

## Status

This project is under active development, but it has been successfully tested across multiple host environments.

Validated install paths currently include:

- Debian with GNU coreutils
- Ubuntu with uutils
- Alpine with BusyBox

Testing has been completed in both virtual machines and on real hardware. Additional testing on more hardware, firmware, and storage layouts is still encouraged.

## What it does

LFS-AI automates major parts of a Linux From Scratch build and installation process, including:

- validating host requirements
- preparing and validating configuration
- partitioning and formatting target disks
- mounting the target layout
- downloading source packages
- building the toolchain and system in phases
- performing final system setup steps

## Important warning

This project can partition disks, format filesystems, and overwrite data. Review your configuration carefully before running any destructive step.

## Requirements

Before using LFS-AI, make sure you have:

- a compatible Linux host system
- the required host dependencies installed
- reviewed [`DEPS.md`](docs/DEPS.md)
- reviewed [`WORKFLOW.md`](docs/WORKFLOW.md)
- created and reviewed `settings.conf` using the LFS-AI configuration step

## Quick start

Clone the repository:

```bash
git clone https://github.com/Vercety1534/lfs-ai.git
cd lfs-ai
```

Review the documentation:

```bash
less docs/DEPS.md
less docs/WORKFLOW.md
```

Start LFS-AI as root when running installation steps:

```bash
sudo ./lfs-ai
```

The menu has the following options:

```text
1) Verify host dependencies
2) Configure settings
3) Prepare host
4) Download sources
5) Build toolchain
6) Build chroot
7) Build Linux from Scratch
8) Build optional packages
9) Build kernel & setup system
a) Run all
q) Quit
```

See [`WORKFLOW.md`](docs/WORKFLOW.md) for the recommended install workflow.

## Logs

Installer logs are written under the target LFS directory during the build process. Review logs after each major step instead of assuming success.

The main log directory is `$LFS/var/log/lfs-ai`.

## Post-install verification

After the first boot into the new LFS system, log in as root and run:

```bash
lfs-ai-verify
```

This checks the installed system state and writes a troubleshooting log to:

```text
/var/log/lfs-ai-verify-YYYY-MM-DD_HH-MM-SS.log
```

The verifier checks items such as:

- boot mode and Limine boot files
- mounted filesystems and swap
- hostname, locale, and console settings
- systemd service health
- ethernet networking and DNS
- boot warnings and kernel messages

If something does not work as expected, include the verification log when reporting an issue.

## Documentation

- [`DEPS.md`](docs/DEPS.md) — host dependency requirements
- [`WORKFLOW.md`](docs/WORKFLOW.md) — recommended install workflow
- [`settings.conf.example`](docs/settings.conf.example) — example configuration file
- [`package.template`](docs/package.template) — package script template

## Testing and feedback

LFS-AI has been tested on Debian with GNU coreutils, Ubuntu with uutils, and Alpine with BusyBox, across both virtual machines and real hardware.

Testing on additional hardware, firmware setups, storage layouts, and virtualized environments is still very helpful. If you test LFS-AI, documenting what worked, what failed, and what hardware or VM platform you used will help make the project stronger and more reliable.

For the most predictable results, use a stable host distribution. During testing, random compiler internal errors and segmentation faults occurred on a rolling-release host, while the same build completed successfully on a clean Debian system on the same hardware.

## Current limitations

- Active development means workflow details may still change
- UEFI/EFI systems are currently required for the full automated install flow
- Users should verify every destructive step before running
- Additional validation on more hardware and host distributions is still encouraged

## License

This project is released under the MIT License. See [`LICENSE`](LICENSE) for the full text.
