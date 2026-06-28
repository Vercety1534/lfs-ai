# WORKFLOW

This document explains the recommended workflow for using LFS-AI from a fresh clone to an install run.

## Clone the repository

```bash
git clone https://github.com/Vercety1534/lfs-ai.git
cd lfs-ai
```

## Review the documentation

Before running the installer, review the project documentation:

```bash
less README.md
less docs/DEPS.md
less docs/WORKFLOW.md
```

At minimum, make sure you understand:

- what the installer does
- which steps are destructive
- which host tools are required
- what values must be set in `settings.conf`

## Start LFS-AI

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

## Choose an install path

LFS-AI can be used in two main ways:

- Full workflow: use `a) Run all` to run the complete guided install flow from host dependency verification through final system setup.
- Phase-based workflow: run individual menu options when testing, debugging, or resuming a specific stage.

For a normal install, use the full workflow.

## Run the full workflow

For a normal install, select:

```text
a) Run all
```

This option runs the complete install workflow, including:

1. verifying host dependencies
2. configuring and validating `settings.conf`
3. preparing the target disk
4. downloading sources
5. building the toolchain
6. building the chroot environment
7. building Linux From Scratch
8. building optional packages
9. building the kernel and final system setup

You do not need to run menu options `1` or `2` first when using `a) Run all`. They are included in the full workflow.

Use the full workflow only after you have:

- reviewed the documentation
- understood which steps are destructive
- confirmed you are ready for the installer to configure settings and erase the selected target disk

## Use a phase-based workflow when needed

The numbered menu options are intended for testing, recovery, or manually running one stage at a time.

Typical manual progression is:

1. `1) Verify host dependencies`
2. `2) Configure settings`
3. `3) Prepare host`
4. `4) Download sources`
5. `5) Build toolchain`
6. `6) Build chroot`
7. `7) Build Linux from Scratch`
8. `8) Build optional packages`
9. `9) Build kernel & setup system`

Use this approach when you want to inspect logs between stages, repeat a specific step, or debug a failed install.

Do not re-run destructive steps casually. In particular, the prepare step should always be reviewed carefully before repeating it.

## Watch output and logs carefully

Do not assume success just because a command started correctly.

Pay close attention to:

- configuration validation
- dependency checks
- partitioning and formatting steps
- mount status
- source downloads
- build failures
- final system setup

Review logs and command output as each stage completes.

Installer logs are written under the target LFS directory during the build process, typically under `$LFS/var/log/lfs-ai`.

## Verify the installed system after first boot

After the installation completes and you boot into the new LFS system for the first time, log in as root and run:

```bash
lfs-ai-verify
```

This produces a post-install verification report and saves a log under `/var/log`.

Recommended checks after first boot:

- confirm the system boots in UEFI mode
- confirm `/`, `/boot`, and swap are active as expected
- confirm the hostname, locale, and keymap are correct
- confirm `systemd-networkd` and `systemd-resolved` are active
- review any reported warnings or failures before treating the install as complete

If you are testing LFS-AI and sharing feedback, include the `lfs-ai-verify` log along with your hardware or VM details.

## Test carefully

This project is still under active development, but it has been validated across multiple host environments.

Confirmed install paths currently include:

- Debian with GNU coreutils
- Ubuntu with uutils
- Alpine with BusyBox

Testing has been completed in both virtual machines and on real hardware.

Even with that coverage, each run should still be treated carefully, especially on destructive steps or new hardware and firmware combinations.

Recommended test environments include:

- spare hardware
- separate drives
- virtual machines
- non-production systems

VM testing is supported and encouraged, but the project is not limited to VM-only use.

## Share results

If you test LFS-AI, useful feedback includes:

- host distribution and version
- host userland/tool implementation, if unusual (for example GNU coreutils, uutils, or BusyBox)
- firmware type
- storage layout
- whether the run was on hardware or in a VM
- where the workflow failed, if it failed
- logs or exact error messages

That kind of feedback is especially valuable while the project continues to be validated on more systems.

## Re-run with caution

Some steps may be safe to re-run, but the prepare step should always be reviewed carefully before repeating it.

Always confirm `settings.conf` and the selected target disk after a reboot.
