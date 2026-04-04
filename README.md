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
- preparing and checking configuration
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
- reviewed [`DEPS.md`](DEPS.md)
- reviewed [`WORKFLOW.md`](WORKFLOW.md)
- reviewed [`settings.conf`](settings.conf) carefully and verified its values

## Root privileges

Most installer actions require root privileges.

In practice, you should expect to run disk preparation, mount, chroot, build, and install steps as root. Review the help output first, then run the required workflow steps with the appropriate privileges.

## Quick start

Clone the repository:

```bash
git clone https://github.com/Vercety1534/lfs-ai.git
cd lfs-ai
```

Review the documentation:

```bash
less DEPS.md
less WORKFLOW.md
```

Check host dependencies:

```bash
./install -v
```

Review, edit, and validate your settings:

```bash
./install -s
```

This step opens [`settings.conf`](settings.conf) in an editor, reloads it, and validates the parsed configuration values.

Run the installer help menu:

```bash
./install -h
```

Run the automated installer:

```bash
sudo ./install -A
```

See [`WORKFLOW.md`](WORKFLOW.md) for the recommended phase-based workflow.

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

- [`README.md`](README.md) — project overview and getting started
- [`DEPS.md`](DEPS.md) — host dependency requirements
- [`WORKFLOW.md`](WORKFLOW.md) — recommended phase-based workflow
- [`settings.conf`](settings.conf) — main configuration file

## Testing and feedback

LFS-AI has been tested on Debian with GNU coreutils, Ubuntu with uutils, and Alpine with BusyBox, across both virtual machines and real hardware.

Testing on additional hardware, firmware setups, storage layouts, and virtualized environments is still very helpful. If you test LFS-AI, documenting what worked, what failed, and what hardware or VM platform you used will help make the project stronger and more reliable.

For the most predictable results, use a stable host distribution. During testing, random compiler internal errors and segmentation faults occurred on a rolling-release host, while the same build completed successfully on a clean Debian system on the same hardware.

## Current limitations

- Active development means workflow details may still change
- Users should verify every destructive step before running
- Additional validation on more hardware and host distributions is still encouraged

## License

This project is released under the MIT License. See [`LICENSE`](LICENSE) for the full text.
