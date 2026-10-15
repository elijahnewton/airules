# Installing OpenCode on Termux: a reliable guide

This guide installs the native `opencode` package on Android using Termux.

The recommended setup is:

- Termux from **F-Droid**
- Android 9/API 28 or newer
- 200 MB or more free storage
- A 64-bit ARM phone, shown as `aarch64`

Do **not** use the Google Play version of Termux for this guide. The F-Droid and official GitHub versions are the most compatible with normal Termux packages.

---

## 1. Install the correct Termux version

Install Termux from F-Droid or the official Termux GitHub releases.

After opening Termux for the first time, check that it is using the normal `apt` package manager:

```bash
echo "$TERMUX_APK_RELEASE"
echo "$TERMUX_APP_PACKAGE_MANAGER"
```

A normal F-Droid installation should show something similar to:

```text
F_DROID
apt
```

You can also check the architecture:

```bash
dpkg --print-architecture
```

For most modern Android phones, the result should be:

```text
aarch64
```

---

## 2. Update Termux completely

Run:

```bash
apt update
apt full-upgrade -y
```

Install the tools needed for installation:

```bash
apt install -y curl ca-certificates
```

Confirm that `curl` works:

```bash
curl --version
```

You should see the curl version instead of a linker error.

### Important: do not partially upgrade Termux

Always update Termux with:

```bash
apt update
apt full-upgrade -y
```

Avoid mixing package versions from different Termux installations or repositories. A partial upgrade can cause errors such as:

```text
CANNOT LINK EXECUTABLE "curl"
cannot locate symbol ...
```

If `curl` shows that error, repair the base installation before continuing:

```bash
unset LD_LIBRARY_PATH
unset LD_PRELOAD

apt update
apt full-upgrade -y
apt install --reinstall -y curl libcurl
```

Then test again:

```bash
curl --version
```

---

## 3. Try the OpenCode installer

The project provides an installer for its Termux repository:

```bash
curl -fsSL https://hope2333.github.io/repo/install.sh | sh -s -- --install opencode
```

This installer detects the architecture and package manager, then attempts to configure the repository and install OpenCode.

If it completes successfully, verify the installation:

```bash
opencode --version
```

Start OpenCode with:

```bash
opencode
```

---

# If the installer fails with a read-only `/etc/apt` error

On some plain `apt` Termux installations, the installer may download a helper package called:

```text
hope2333-mirrorlist
```

It may then fail with an error like:

```text
error creating directory './etc/apt': Read-only file system
```

This happens because Termux does not use a normal Linux root filesystem. Termux’s writable system prefix is:

```text
/data/data/com.termux/files/usr
```

The Android root-level `/etc` directory is not writable by a normal Termux process.

In that case, do not keep retrying the installer. Install the OpenCode `.deb` package directly.

---

## 4. Remove the failed helper package

Run:

```bash
dpkg --purge hope2333-mirrorlist 2>/dev/null || true
```

This is safe even if the package was only partially installed.

You can check whether it is still present:

```bash
dpkg -l | grep hope2333
```

If there is no output, it has been removed.

---

## 5. Download the native OpenCode package

The native package uses this general filename format:

```text
opencode_VERSION_aarch64.deb
```

For example:

```text
opencode_2.0.22_aarch64.deb
```

Download the package with:

```bash
cd ~

curl -fL --retry 3 \
  -o opencode_2.0.22_aarch64.deb \
  https://github.com/Hope2333/opencode-termux/releases/latest/download/opencode_2.0.22_aarch64.deb
```

If the filename changes in a newer release, open the project’s Releases page and copy the exact filename for:

```text
opencode_<version>_aarch64.deb
```

Do not choose:

- `opencode1`
- `opencode-wrapper`
- `opencode1-wrapper`
- compressed packages
- packages for another architecture

For this guide, choose the regular native package named:

```text
opencode_<version>_aarch64.deb
```

---

## 6. Verify the downloaded file

First check that the file exists:

```bash
ls -lh ~/opencode_*.deb
```

You can inspect the package metadata:

```bash
dpkg-deb --info ~/opencode_2.0.22_aarch64.deb
```

For stronger verification, compare the SHA-256 checksum shown on the GitHub release page.

Example:

```bash
sha256sum ~/opencode_2.0.22_aarch64.deb
```

Compare the output with the checksum published for that exact release asset.

---

## 7. Install OpenCode manually

Install the package with:

```bash
dpkg -i ~/opencode_2.0.22_aarch64.deb
```

If installation completes successfully, run:

```bash
opencode --version
```

Then start OpenCode:

```bash
opencode
```

---

## 8. If `dpkg` reports missing dependencies

Run:

```bash
apt --fix-broken install -y
```

Then reinstall the OpenCode package:

```bash
dpkg -i ~/opencode_2.0.22_aarch64.deb
```

Verify again:

```bash
opencode --version
```

---

# Complete installation sequence

For a clean F-Droid Termux installation, the normal sequence is:

```bash
apt update
apt full-upgrade -y
apt install -y curl ca-certificates
curl --version
curl -fsSL https://hope2333.github.io/repo/install.sh | sh -s -- --install opencode
opencode --version
```

If the installer fails with the `/etc/apt` read-only error, use this fallback:

```bash
dpkg --purge hope2333-mirrorlist 2>/dev/null || true

cd ~

curl -fL --retry 3 \
  -o opencode_2.0.22_aarch64.deb \
  https://github.com/Hope2333/opencode-termux/releases/latest/download/opencode_2.0.22_aarch64.deb

dpkg -i ~/opencode_2.0.22_aarch64.deb

apt --fix-broken install -y

dpkg -i ~/opencode_2.0.22_aarch64.deb

opencode --version
```

---

# Updating OpenCode later

First update the Termux package lists:

```bash
apt update
```

If OpenCode was successfully registered as an `apt` package, try:

```bash
apt upgrade opencode
```

or:

```bash
apt install --only-upgrade opencode
```

If the repository installer continues to fail because of the mirror-list package, download the newer `.deb` manually and install it:

```bash
dpkg -i ~/opencode_NEW_VERSION_aarch64.deb
```

Replace `NEW_VERSION` with the actual version downloaded.

---

# Useful commands

Check whether OpenCode is installed:

```bash
command -v opencode
```

Show the installed package version:

```bash
dpkg-query -W opencode
```

Show the executable location:

```bash
which opencode
```

Check the binary:

```bash
file "$(command -v opencode)"
```

Start OpenCode:

```bash
opencode
```

Display help:

```bash
opencode --help
```

---

# Common mistakes

## Mistake 1: Using the old official installer URL

This command may return `404`:

```bash
curl -fsSL https://opencode.ai/install.sh | bash
```

Use the Termux project installer instead:

```bash
curl -fsSL https://hope2333.github.io/repo/install.sh | sh -s -- --install opencode
```

If that installer fails on the mirror-list package, use the direct `.deb` fallback.

## Mistake 2: Building from source unnecessarily

Do not run these for a normal installation:

```bash
make build-native VER=2.0.0
make deb-native VER=2.0.0
```

Those are maintainer/build commands. They require additional source-build files and runtimes, including Android Bun artifacts.

You do not need to clone the repository or build OpenCode from source.

## Mistake 3: Pasting command output as commands

Text such as this is output:

```text
architecture: aarch64
warning: ...
Subscribed repositories:
```

Do not paste it back into the shell. Only enter actual commands shown in code blocks.

## Mistake 4: Mixing Termux versions

Do not mix packages or files from:

- Google Play Termux
- F-Droid Termux
- GitHub Termux
- pacman-based Termux forks

Install Termux and its add-ons from the same source.

## Mistake 5: Installing the wrong package family

For the standard recommended installation, use:

```text
opencode_<version>_aarch64.deb
```

The regular `opencode` package is the native v2 mainline package. The other package names are alternate generations or runtime variants.

---

# Recommended final test

After installation, run:

```bash
command -v opencode
opencode --version
opencode --help
```

If all three commands work, OpenCode is installed correctly. You can then launch it from any directory with:

```bash
opencode
```