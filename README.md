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

* Fetch [Linux 6.5.6](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v6.5.6) source code.
 
```bash
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.5.6 &&
sudo mv linux /usr/src/linux-6.5.6-shinobu
```

## Preparation Before Compilation

* Before starting please read [Minimal requirements to compile the Kernel](https://www.kernel.org/doc/html/latest/process/changes.html)

* Fetch kernel configuration.

```bash
git clone https://github.com/abdul-royyaq/shinobu-x86_64.git --depth 1 &&
sudo mv shinobu-x86_64 /usr/src/shinobu-x86_64
```

* Backup original linux logo & apply kernel configuration.

```bash
mv /usr/src/linux-6.5.6-shinobu/drivers/video/logo/logo_linux_clut224.ppm /usr/src/linux-6.5.6-shinobu/drivers/video/logo/logo_linux_clut224.backup.ppm &&
cp /usr/src/shinobu-x86_64/.config /usr/src/linux-6.5.6-shinobu/ &&
cp /usr/src/shinobu-x86_64/logo/logo_linux_clut224-1920x1080.ppm /usr/src/linux-6.5.6-shinobu/drivers/video/logo/logo_linux_clut224.ppm
```

`*For 1366x768p resolution can use logo_linux_clut224-1366x768.ppm`

```bash
cp /usr/src/shinobu-x86_64/logo/logo_linux_clut224-1366x768.ppm /usr/src/linux-6.5.6-shinobu/drivers/video/logo/logo_linux_clut224.ppm
```

* Entering kernel source directory.

```bash
cd /usr/src/linux-6.5.6-shinobu
```

* Modify kernel configuration (optional).

Modify kernel configuration if you wish.

```bash
make menuconfig 
```

## Kernel Compilation

* Start Compilation.

Kernel compilation with all cpu cores.

```bash
make LOCALVERSION= -j$(nproc)
```

`*Added 'LOCALVERSION=' flag at compile time to avoid weird names added due to updates after commit tag`

## Kernel Installation

* Kernel installation.

Kernel installation.

```bash
sudo make -j$(nproc) modules_install &&
sudo make -j$(nproc) install
```

`*Kernel installation if using GRUB2.`

```bash
sudo make -j$(nproc) modules_install &&
sudo cp arch/x86/boot/bzImage /boot/vmlinuz-6.5.6-shinobu-x86_64 &&
sudo cp System.map /boot/System.map-6.5.6-shinobu-x86_64
```

## Kernel Documentation

* Install kernel documentation (optional).

```bash
sudo install -d /usr/share/doc/linux-6.5.6-shinobu-x86_64 &&
sudo cp -r Documentation/* /usr/share/doc/linux-6.5.6-shinobu-x86_64
```

## Generate Initramfs

* Generate initramfs image.

Ways to generate initramfs image.

`- Minimal using mkinitcpio.`

```bash
sudo mkinitcpio -z lz4 -k 6.5.6-shinobu-x86_64 -g /boot/initramfs-6.5.6-shinobu-x86_64.img
```

`- Full using dracut.`

```bash
sudo dracut --lz4 --kver 6.5.6-shinobu-x86_64 /boot/initramfs-6.5.6-shinobu-x86_64.img
```

## Updating GRUB2 Bootloader

GRUB2 bootloader is used in almost every modern Linux Distro's.

* Generate GRUB2 configuration.

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

`*In some distro's.`

``` 
grub2-mkconfig -o /boot/grub/grub.cfg
```

---
