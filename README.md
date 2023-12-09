# Shinobu-x86_64

Shinobu-x86_64 is a personal Linux kernel configuration based on the latest stable [Linux kernel](https://kernel.org) release.
Customized for high performance stability and security with low latency. Offers high performance, stability, security, and responsiveness for desktop and gaming experiences.

### Features Offered

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

## Fetch Linux Kernel Source

* Fetch [Linux 6.6.5](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v6.6.5) source code.
 
```bash
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.6.5 &&
sudo mv linux /usr/src/linux-6.6.5-shinobu
```

## Preparation Before Compilation

* Before starting please read [Minimal requirements to compile the Kernel](https://www.kernel.org/doc/html/latest/process/changes.html)

* Fetch kernel configuration.

```bash
git clone https://github.com/abdul-royyaq/Shinobu-x86_64.git --depth 1 &&
sudo mv Shinobu-x86_64 /usr/src/Shinobu-x86_64
```

* Backup original linux logo & apply kernel configuration.

```bash
mv /usr/src/linux-6.6.5-shinobu/drivers/video/logo/logo_linux_clut224.ppm /usr/src/linux-6.6.5-shinobu/drivers/video/logo/logo_linux_clut224.backup.ppm &&
cp /usr/src/Shinobu-x86_64/.config /usr/src/linux-6.6.5-shinobu/ &&
cp /usr/src/Shinobu-x86_64/logo/logo_linux_clut224-1920x1080.ppm /usr/src/linux-6.6.5-shinobu/drivers/video/logo/logo_linux_clut224.ppm
```

`*For 1366x768p resolution can use logo_linux_clut224-1366x768.ppm`

```bash
cp /usr/src/Shinobu-x86_64/logo/logo_linux_clut224-1366x768.ppm /usr/src/linux-6.6.5-shinobu/drivers/video/logo/logo_linux_clut224.ppm
```

* Entering kernel source directory.

```bash
cd /usr/src/linux-6.6.5-shinobu
```

* Modify kernel configuration if you wish (optional).

```bash
make menuconfig 
```

## Kernel Compilation

* Start Compilation.

Kernel compilation with all cpu cores

```bash
make LOCALVERSION= -j$(nproc)
```

`*Added 'LOCALVERSION=' flag at compile time to avoid weird names added due to updates after commit tag.`

## Kernel Installation

* Kernel installation.

```bash
sudo make -j$(nproc) modules_install &&
sudo cp arch/x86/boot/bzImage /boot/vmlinuz-6.6.5-shinobu-x86_64 &&
sudo cp System.map /boot/System.map-6.6.5-shinobu-x86_64
```

## Kernel Documentation

* Install kernel documentation (optional).

```bash
sudo install -d /usr/share/doc/linux-6.6.5-shinobu-x86_64 &&
sudo cp -r Documentation/* /usr/share/doc/linux-6.6.5-shinobu-x86_64
```

## Generate Initramfs

* Generate minimal initramfs using mkinitcpio.

```bash
sudo mkinitcpio -z lz4 -k 6.6.5-shinobu-x86_64 -g /boot/initramfs-6.6.5-shinobu-x86_64.img
```

* Generate full initramfs using dracut.

```bash
sudo dracut --lz4 --kver 6.6.5-shinobu-x86_64 /boot/initramfs-6.6.5-shinobu-x86_64.img
```

`*2 ways to generate initramfs, use one (minimal / full).`

## Update Bootloader

* Generate GRUB2 configuration.

GRUB2 bootloader is used in almost every modern Linux Distro's.

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg || sudo grub2-mkconfig -o /boot/grub/grub.cfg
```

`*In some distro's, 'grub-mkconfig' command is not found and use 'grub2-mkconfig' instead.`

## Uninstall Kernel

* Uninstall kernel.

```bash
sudo rm -r /boot/*6.6.5-shinobu-x86_64* &&
sudo rm -r /lib/modules/6.6.5-shinobu-x86_64 &&
sudo rm -r /usr/share/doc/linux-6.6.5-shinobu-x86_64 &&
sudo grub-mkconfig -o /boot/grub/grub.cfg || sudo grub2-mkconfig -o /boot/grub/grub.cfg
```

`*Before uninstall this kernel, make sure there is another kernel installed.`

## License

This project is licensed under the [GPL v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html), material inside the logo folder is licensed under [CC0](https://creativecommons.org/publicdomain/zero/1.0), and The Linux Kernel is licensed under [GPL v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html).

---
