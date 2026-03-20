# WORKFLOW

This document explains the recommended workflow for using LFS-AI from a fresh clone to a full install run.

## Clone the repository

```bash
git clone https://github.com/Vercety1534/lfs-ai.git
cd lfs-ai
```

## Review the documentation

Before running the installer, review the project documentation:

```bash
less README.md
less DEPS.md
less WORKFLOW.md
```

At minimum, make sure you understand:
- what the installer does
- which steps are destructive
- which host tools are required
- what values must be set in [`settings.conf`](settings.conf)

## Verify host dependencies

Run the built-in dependency check:

```bash
./install -v
```

If dependency verification reports missing packages, install them on the host system before continuing.

You should also review [`DEPS.md`](DEPS.md) if needed.

## Review and edit `settings.conf`

Check [`settings.conf`](settings.conf) carefully before doing anything destructive.

You can review it with:

```bash
./install -s
```

Confirm all required values are correct, especially the target disk and any installation-specific settings.

Do not continue until you are sure the selected drive and layout are correct.

## Review installer options

```bash
./install -h
```

This shows the currently supported flags, actions, and workflow options.

If the installer behavior changes over time, the help output should be treated as the source of truth.

Current installer help output:

```text
Usage: ./install [-A] [-v] [-h] [-s] [-p] [-d] [-t] [-c] [-l 1|2|3|4|5|all] [-f] [--version]
    -A        :  ALL   :   Complete install including drive partitioning
    -v        :  DEPS  :   Verify dependencies on host system
    -h        :  Help  :   Display help message

    -s        : Step 1 :   Edit settings.conf (IMPORTANT - Failure to do so could destroy your system)
    -p        : Step 2 :   Prepare the host system and partition the drive
    -d        : Step 3 :   Download the required source packages
    -t        : Step 4 :   Install the toolchain
    -c        : Step 5 :   Prepare the chroot environment
    -l #      : Step 6 :   Install LFS [Phases - 1|2|3|4|5|all]
    -f        : Step 7 :   Install kernel, config files, and clean up build system

    --version :        :   Display installer version
```

## Run the installer

To run the automated workflow:

```bash
./install -A
```

Use the automated mode only after:
- reviewing the documentation
- confirming host dependencies
- verifying [`settings.conf`](settings.conf)

## Use a phase-based workflow when needed

LFS-AI is designed around staged execution. Depending on your testing or recovery needs, you may want to run the project in phases instead of treating it as a single one-shot install.

Typical workflow progression is:

1. review and verify [`settings.conf`](settings.conf)
2. prepare the host system and partition the drive
3. download the required source packages
4. install the toolchain
5. prepare the chroot environment
6. install LFS phases
7. install the kernel, config files, and perform final cleanup

Refer to `./install -h` and the repository scripts for the currently supported phase controls.

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

## Test carefully

This project is still under active development and has only been tested on one system so far.

Treat each run as a test run until broader validation exists.

Recommended test environments include:

- spare hardware
- separate drives
- virtual machines
- non-production systems

VM testing is supported and encouraged, but the project is not limited to VM-only use.

## Share results

If you test LFS-AI, useful feedback includes:

- host distribution and version
- firmware type
- storage layout
- whether the run was on hardware or in a VM
- where the workflow failed, if it failed
- logs or exact error messages

That kind of feedback is especially valuable while the project is still being validated on more systems.

## Re-run with caution

Some steps may be safe to re-run, but `./install -p` should always be reviewed carefully before repeating it.

Always confirm [`settings.conf`](settings.conf) after a reboot. Systems do not always enumerate drives in the same order.
