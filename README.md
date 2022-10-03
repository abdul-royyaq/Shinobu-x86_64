# Shinobu-x86_64

Shinobu-x86_64 is a personal custom kernel configuration based on the latest stable [kernel](https://kernel.org) release.
Customized for high performance stability with low latency. offering stability, high performance, and responsiveness for desktop and gaming experience.

## Fetching Linux kernel source

Fetching [linux-6.0](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?h=v6.0) source code.
 
```bash
# Using Git
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.0 linux-6.0

# Using Wget
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.0.tar.xz && tar -xf linux-6.0.tar.xz
```
Fetching patch and applying.

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/patch-6.0.xz && xz -d patch-6.0.xz && patch -d linux-6.0 -p1 < patch-6.0
```

## Kernel Compilation

Copy the kernel configuration first before starting compilation.

```bash
# If you need to configure
make menuconfig 

# Kernel compilation (with all cpu cores)
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

* Method 2 Manually (if Grub2 bootloader not detect new installed kernel)

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
cp -iv arch/x86/boot/bzImage /boot/vmlinuz-6.0-shinobu-x86_64

# Install System.map
cp -iv System.map /boot/System.map-6.0-shinobu-x86_64
```
## Install Kernel Documentation (Optional)

If you want documentation for the linux kernel.

```bash
# Install kernel documentation (optional)
install -d /usr/share/doc/linux-6.0-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-6.0-shinobu-x86_64
```

## Generate Initramfs (Optional)

To generate a minimal initramfs, you can use [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio/Minimal_initramfs) instead.

```bash
# Generate initramfs (optional)
dracut --kver 6.0-shinobu-x86_64 /boot/initramfs-6.0-shinobu-x86_64.img --force
```

## Updating Grub2 Bootloader

```bash
# Update Grub2
update-grub

# Incase update-grub command not available
grub-mkconfig -o /boot/grub/grub.cfg
```