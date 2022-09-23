# linux-5.19.10-shinobu-x86_64

Shinobu-x86_64 is a personal custom kernel configuration based on the latest stable [kernel](https://kernel.org) release.
Customized for high performance stability with low latency. offering stability, high performance, and responsiveness for desktop and gaming experience.

## Kernel Compilation

Copy the kernel configuration first before starting compilation.

```bash
# If you need to configure
make menuconfig 

# Kernel compilation (with all cores cpu)
make -j$(nproc)
```

## Kernel Installation

There are two methods to install the kernel.

* Method 1

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
make -j$(nproc) install
```

* Method 2 (Manually)

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
cp -iv arch/x86/boot/bzImage /boot/vmlinuz-5.19.10-shinobu-x86_64

# Install System.map
cp -iv System.map /boot/System.map-5.19.10-shinobu-x86_64
```
## Install Kernel Documentation (Optional)

If you want documentation for the linux kernel.

```bash
# Install kernel documentation (optional)
install -d /usr/share/doc/linux-5.19.10-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-5.19.10-shinobu-x86_64
```

## Generate Initramfs (Optional)

To generate a minimal initramfs, you can use [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio/Minimal_initramfs) instead.

```bash
# Generate initramfs (optional)
dracut --kver 5.19.10-shinobu-x86_64 /boot/initramfs-5.19.10-shinobu-x86_64.img --force
```