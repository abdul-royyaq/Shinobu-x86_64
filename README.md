# Shinobu-x86_64

Shinobu-x86_64 is a personal Linux kernel configuration based on the latest stable [Linux kernel](https://kernel.org) release.
Customized for high performance stability with low latency. offers stability, high performance, and responsiveness for desktop and gaming experiences.

### Features Offered

* Performance as default cpufreq governor.
* Low latency desktop preemption.
* Fast timer frequency 1000hz.
* -O2 level compiler optimizations.
* Uses the fastest LZ4 compression.

---

## Fetching Linux Kernel Source

Fetching [linux-6.3.0](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v6.3) source code.
 
```bash
# Fetching kernel source and place in /usr/src directory
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.3 /usr/src/linux-6.3.0-shinobu
```

## Preparation Before Compilation

Fetching this kernel configuration.

```bash
#Fetching this kernel configuration and place in /usr/src directory
git clone https://github.com/abdul-royyaq/shinobu-x86_64.git /usr/src/Shinobu-x86_64
```

Copy the kernel configuration first before starting compilation.

```bash
# Backup original logo_linux_clut224.ppm and copy kernel configuration
mv /usr/src/linux-6.3.0-shinobu/drivers/video/logo/logo_linux_clut224.ppm /usr/src/linux-6.3.0-shinobu/drivers/video/logo/logo_linux_clut224.backup.ppm && cp -r /usr/src/Shinobu-x86_64/{.config,drivers,localversion} /usr/src/linux-6.3.0-shinobu
```

Entering kernel source directory.

```bash
# Moving to kernel source directory 
cd /usr/src/linux-6.3.0-shinobu
```

If need adjust the configuration (optional).

```bash
# If you need to configure
make menuconfig 
```

## Kernel Compilation

Start Compilation

```bash
# Kernel compilation (with all cpu cores), add the `LOCALVERSION=` flag at compile time to avoid the `+` that being added to the name automatically in updated source code
make LOCALVERSION= -j$(nproc)
```

## Kernel Installation

* Kernel installation

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
make -j$(nproc) install
```

* Kernel installation if using Grub2 bootloader

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
cp arch/x86/boot/bzImage /boot/vmlinuz-6.3.0-shinobu-x86_64

# Install System.map
cp System.map /boot/System.map-6.3.0-shinobu-x86_64
```

## Install Kernel Documentation (Optional)

If you want documentation for the Linux kernel.

```bash
# Install kernel documentation (optional)
install -d /usr/share/doc/linux-6.3.0-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-6.3.0-shinobu-x86_64
```

## Generate Initramfs (Optional)

* Generate full initramfs image using dracut.

```bash
# Generate full initramfs image using dracut
dracut --kver 6.3.0-shinobu-x86_64 /boot/initramfs-6.3.0-shinobu-x86_64.img
```

* Generate minimal initramfs using mkinitcpio.

```bash
# Generate minimal initramfs using mkinitcpio
mkinitcpio -k 6.3.0-shinobu-x86_64 -g /boot/initramfs-6.3.0-shinobu-x86_64.img
```

## Updating Grub2 Bootloader

Grub2 bootloader is used in almost every modern Linux Distro's, you don't need it if you're using LiLo, or read here if you are using [other bootloaders](https://wiki.archlinux.org/title/Category:Boot_loaders).

```bash
# Update Grub2
update-grub

# Incase update-grub command not available
grub-mkconfig -o /boot/grub/grub.cfg
# or
grub2-mkconfig -o /boot/grub/grub.cfg
```

---
