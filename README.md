# Shinobu-x86_64

Shinobu-x86_64 is a personal Linux kernel configuration based on the latest stable [Linux kernel](https://kernel.org) release.
Customized for high performance stability and security with low latency. Offers high performance, stability, security, and responsiveness for desktop and gaming experiences.

## Features Offered

* Performance cpufreq governor.
* Performance PCIe ASPM policy.
* Security kernel hardening.
* SELinux & Apparmor isolation support.
* Low latency desktop preemption.
* Fast 1000hz timer frequency.
* Fastest LZ4 compression.
* Device driver as kernel module.
* -O2 level for compiler optimization.

---

## Preparation Before Compilation

### Install Tools for Build and Compilation

To build the Linux kernel, you need this software installed on your system.

* Debian/Ubuntu

```bash
sudo apt install git build-essential bc bison flex libncurses-dev libssl-dev libelf-dev lz4 zstd cpio
```

* Arch/Manjaro

```bash
sudo pacman -S git base-devel bc ncurses openssl libelf lz4 zstd cpio
```

* Fedora

```bash
sudo dnf install git @development-tools bc ncurses-devel lz4 zstd cpio
```

### Set Environment Variable

Run this command every time you open a new shell session.

```bash
kver=6.8.0-shinobu
```

### Fetch Linux Kernel Source

Fetch [Linux 6.8](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v6.8) source code.
 
```bash
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.8
sudo mv linux /usr/src/linux-$kver
```

### Fetch kernel configuration

```bash
git clone https://github.com/abdul-royyaq/Shinobu-x86_64.git --depth 1
sudo mv Shinobu-x86_64 /usr/src/Shinobu-x86_64
```

### Apply kernel configuration

Backup original linux logo & apply kernel configuration.

```bash
mv /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.backup.ppm
cp /usr/src/Shinobu-x86_64/.config /usr/src/linux-$kver/
cp /usr/src/Shinobu-x86_64/logo/logo_linux_clut224-1920x1080.ppm /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm
```

**For 1366x768p resolution can use `logo_linux_clut224-1366x768.ppm`.*

```bash
cp /usr/src/Shinobu-x86_64/logo/logo_linux_clut224-1366x768.ppm /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm
```

### Entering kernel source directory

```bash
cd /usr/src/linux-$kver
```

### Modify kernel configuration

Modify kernel configuration if you wish (optional).

```bash
make menuconfig
```

## Kernel Compilation

Kernel compilation with all CPU cores.

```bash
make LOCALVERSION= -j$(nproc)
```

**Added `LOCALVERSION=` flag at compile time to avoid weird names added due to updates after commit tag.*

## Kernel Installation

Installing Linux kernel.

```bash
sudo make -j$(nproc) modules_install 
sudo cp arch/x86/boot/bzImage /boot/vmlinuz-$kver-x86_64
sudo cp System.map /boot/System.map-$kver-x86_64
```

## Kernel Documentation

Installing kernel documentation (optional).

```bash
sudo install -d /usr/share/doc/linux-$kver-x86_64
sudo cp -r Documentation/* /usr/share/doc/linux-$kver-x86_64
```

## Generate Initramfs

There are two ways to generate initramfs, use one (minimal/full).

### Generate Minimal Initramfs

Generate minimal initramfs using mkinitcpio (mostly used on ArchLinux).

```bash
sudo mkinitcpio -z lz4 -k $kver-x86_64 -g /boot/initramfs-$kver-x86_64.img
```

### Generate Full Initramfs

Generate full initramfs using dracut (mostly used on Debian and Fedora).

```bash
sudo dracut --lz4 --kver $kver-x86_64 /boot/initramfs-$kver-x86_64.img
```

## Update Bootloader

### Generate GRUB2 Configuration

GRUB2 bootloader is used in almost all modern Linux Distros, including Debian, ArchLinux, and Fedora.

* Debian/Ubuntu

```bash
sudo update-grub
```

* Arch/Manjaro

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

* Fedora

```bash
sudo grubby --title="$(cat /etc/os-release | grep 'NAME' | sed -e 's/NAME="\(.*\)"/\1/' | head -1) ($kver-x86_64) $(cat /etc/os-release | grep 'VERSION' | sed -e 's/VERSION="\(.*\)"/\1/' | head -1)"--add-kernel=/boot/vmlinuz-$kver-x86_64 --copy-default
```

## Uninstall Kernel

If you want to uninstall this Linux kernel.

```bash
kver=6.8.0-shinobu
sudo rm -r /boot/*$kver-x86_64*
sudo rm -r /lib/modules/$kver-x86_64
sudo rm -r /usr/share/doc/linux-$kver-x86_64
sudo update-grub || sudo grub-mkconfig -o /boot/grub/grub.cfg || sudo grubby --remove-kernel=/boot/vmlinuz-$kver-x86_64
```

**Before uninstall this kernel, make sure there is another kernel installed.*

## License

This project is licensed under the [GPL v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html), material inside the [logo folder](logo/) is licensed under [CC0](https://creativecommons.org/publicdomain/zero/1.0), Shinobu is a character from [Kimetsu no Yaiba](https://kimetsu.com) written by Koyoharu Gotouge, [The Linux Kernel](https://kernel.org) is licensed under [GPL v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html), and Linux is a registered trademark of Linus Torvalds.

---
