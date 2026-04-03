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

This step opens [`settings.conf`](settings.conf) in an editor, reloads the file, and validates the parsed configuration values.

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
Usage: ./install [-A] [-v] [-h] [-s] [-p] [-d] [-t] [-c] [-l <arg>] [-o] [-f] [--version]
    -A        :  ALL   :   Complete install including drive partitioning
    -v        :  DEPS  :   Verify dependencies on host system
    -h        :  Help  :   Display help message

    -s        : Step 1 :   Edit settings.conf (IMPORTANT - Failure to do so could destroy your system)
    -p        : Step 2 :   Prepare the host system and partition the drive
    -d        : Step 3 :   Download the required source packages
    -t        : Step 4 :   Install the toolchain
    -c        : Step 5 :   Prepare the chroot environment
    -l <arg>  : Step 6 :   Install LFS phases [1|2|3|4|5|all]
    -o        : Step 7 :   Install cmake, fastfetch, and required dependencies (Optional)
    -f        : Step 8 :   Install kernel, config files, and clean up build system

    --version :        :   Display installer version
```

## Run the installer

To run the automated workflow:

```bash
sudo ./install -A
```

Use the automated mode only after:
- reviewing the documentation
- confirming host dependencies
- reviewing, editing, and verifying [`settings.conf`](settings.conf)

## Use a phase-based workflow when needed

LFS-AI is designed around staged execution. Depending on your testing or recovery needs, you may want to run the project in phases instead of treating it as a single one-shot install.

Typical workflow progression is:

1. review and verify [`settings.conf`](settings.conf)
2. prepare the host system and partition the drive
3. download the required source packages
4. install the toolchain
5. prepare the chroot environment
6. install the main LFS phases
7. optionally install cmake, fastfetch, and their required dependencies
8. install the kernel, config files, and perform final cleanup

```bash
./install -s
sudo ./install -p
sudo ./install -d
sudo ./install -t
sudo ./install -c
sudo ./install -l all
sudo ./install -o
sudo ./install -f

# After first boot, as root:
lfs-ai-verify
```

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
