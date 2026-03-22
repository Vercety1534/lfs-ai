# LFS-AI

LFS-AI is a **Linux From Scratch Automated Installer** designed to build and install an LFS system through a guided, script-driven workflow.

## Status

This project is under active development and still needs broader testing. It has only been tested on one system so far, so additional testers and hardware reports are important before it can be considered broadly reliable.

Virtual machine testing is supported and encouraged, but the project is not limited to VM-only use.

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
- reviewed and verified [`settings.conf`](settings.conf) carefully

## Root privileges

Most installer actions require root privileges.

In practice, you should expect to run disk preparation, mount, chroot, build, and install steps as root. Review the help output first, then run the required workflow steps with appropriate privileges.

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

The main log directory is `$LFS/logs`.

## Documentation

- [`README.md`](README.md) — project overview and getting started
- [`DEPS.md`](DEPS.md) — host dependency requirements
- [`WORKFLOW.md`](WORKFLOW.md) — recommended phase-based workflow
- [`settings.conf`](settings.conf) — main configuration file

## Testing and feedback

Testing on additional hardware, firmware setups, storage layouts, and virtualized environments would be very helpful. If you test LFS-AI, documenting what worked, what failed, and what hardware or VM platform you used will help make the project stronger and more reliable.

For the most predictable results, use a stable host distribution. During testing, random compiler internal errors and segmentation faults occurred on a rolling-release host, while the same build completed successfully on a clean Debian system on the same hardware.

## Current limitations

- It has only been tested on one system so far.
- Active development means workflow details may still change.
- Users should verify every destructive step before running.

## License

This project is released under the MIT License. See [`LICENSE`](LICENSE) for the full text.
