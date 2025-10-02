# Shinobu-x86_64

Shinobu-x86_64 is a personal Linux kernel configuration based on the latest stable [Linux kernel](https://kernel.org) release.
Customized for high performance stability and security with low latency. Offers high performance, stability, security, and responsiveness for desktop and gaming experiences.

## Features Offered

* Performance as default CPUFreq governor (depends on ACPI & userspace power profile).
* Performance as default PCIe ASPM policy (depends on ACPI power profile).
* Pre-configure security kernel hardening.
* SELinux & Apparmor isolation support (SELinux & Apparmor config depends on your system).
* Low latency desktop preemption model.
* Build with fully ZSTD compression.
* Non-core device driver as kernel module.
* Optimize for fast compilation.
* Extreme debloat, disabling unnecessary kernel modules for desktop & laptop use.

---

## Preparation Before Compilation

### Install Tools for Build and Compilation

To build the Linux kernel, you need this following software installed on your system.

* Debian/Ubuntu

```bash
sudo apt install git build-essential bc bison flex libncurses-dev libssl-dev libelf-dev zstd cpio
```

* Arch/Manjaro

```bash
sudo pacman -S git base-devel bc ncurses openssl libelf zstd cpio
```

* Fedora

```bash
sudo dnf install git @development-tools bc ncurses-devel zstd cpio
```

### Set Environment Variable

Run this command every time you open a new shell session.

```bash
kver=6.17.0-shinobu
```

### Fetch Linux Kernel Source

Fetch [Linux 6.17](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v6.17) source code.
 
```bash
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.17
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
cp /usr/src/Shinobu-x86_64/logo/shinobu_tired-1920x1072.ppm /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm
```

**For 1366x768p resolution can use `shinobu_tired-1366x768.ppm`.*

```bash
cp /usr/src/Shinobu-x86_64/logo/shinobu_tired-1366x768.ppm /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm
```

**You can change the boot splash logo, see [logo](logo/) folder for other Shinobu splash screen.*

**Or [use your own boot logo](logo/README.md)*

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

## Kernel Installation

Installing Linux kernel.

```bash
sudo make -j$(nproc) modules_install
sudo cp arch/x86/boot/bzImage /boot/vmlinuz-$kver-x86_64
```

## Kernel Documentation

Installing kernel documentations (optional).

```bash
sudo cp -r Documentation /usr/share/doc/linux-$kver-x86_64
```

## Generate Initramfs

Generate Initramfs Images.

```bash
sudo mkinitcpio -z zstd -k $kver-x86_64 -g /boot/initramfs-$kver-x86_64.img || sudo dracut --zstd --kver $kver-x86_64
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
sudo grub2-editenv - unset menu_auto_hide
sudo kernel-install add $kver-x86_64 /boot/vmlinuz-$kver-x86_64
```

## Uninstall Kernel

If you want to uninstall this Linux kernel.

```bash
kver=6.17.0-shinobu
sudo rm -r /boot/*$kver-x86_64*
sudo rm -r /lib/modules/$kver-x86_64
sudo rm -r /usr/share/doc/linux-$kver-x86_64
sudo update-grub || sudo grub-mkconfig -o /boot/grub/grub.cfg || sudo kernel-install remove $kver-x86_64
```

**Before uninstall this kernel, make sure there is another kernel installed.*

## License

This project is licensed under the [GPL v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html), material inside the [logo folder](logo/) is licensed under [CC0](https://creativecommons.org/publicdomain/zero/1.0), Shinobu is a character from [Kimetsu no Yaiba](https://kimetsu.com) written by Koyoharu Gotouge, [The Linux Kernel](https://kernel.org) is licensed under [GPL v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html), and Linux is a registered trademark of Linus Torvalds.

---
