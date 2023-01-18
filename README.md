# Shinobu-x86_64

Shinobu-x86_64 is a personal custom kernel configuration based on the latest stable [kernel](https://kernel.org) release.
Customized for high performance stability with low latency. offering stability, high performance, and responsiveness for desktop and gaming experience.

### Features Offered

* Default cpufreq governor performance
* Low latency desktop model preemption
* Fast timer frequency 1000hz
* Compiler optimization level -O2
* Use LZ4 compression

---

## Fetching Linux kernel source

Fetching [linux-6.1.6](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v6.1.6) source code.
 
```bash
# Using Git
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git --depth 1 -b v6.1.6 linux-6.1.6

# Using Wget
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.6.tar.xz && tar -xf linux-6.1.6.tar.xz
```
Fetching patch and applying.

```bash
# Apply each patch manually
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/patch-6.1.6.xz && xz -d patch-6.1.6.xz && patch -d linux-6.1.6 -p1 < patch-6.1.6

# Apply all patches automatically (not recommended if source code from git)
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/patch-6.1.6.xz && xz -d patch-6.1.6.xz && patch -fd linux-6.1.6 -p1 < patch-6.1.6
```

## Kernel Compilation

Copy the kernel configuration first before starting compilation.

```bash
# If you need to configure
make menuconfig 

# Kernel compilation (with all cpu cores)
make -j$(nproc)

# To avoid the `+` sign being added automatically to version names of modified git release sources, add the `LOCALVERSION=` flag at compile time
make LOCALVERSION= -j$(nproc)
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
cp arch/x86/boot/bzImage /boot/vmlinuz-6.1.0-shinobu-x86_64

# Install System.map
cp System.map /boot/System.map-6.1.0-shinobu-x86_64
```
## Install Kernel Documentation (Optional)

If you want documentation for the linux kernel.

```bash
# Install kernel documentation (optional)
install -d /usr/share/doc/linux-6.1.6-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-6.1.6-shinobu-x86_64
```

## Generate Initramfs (Optional)

To generate a minimal initramfs, you can use [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio/Minimal_initramfs) instead.

```bash
# Generate initramfs (optional)
dracut --kver 6.1.6-shinobu-x86_64 /boot/initramfs-6.1.6-shinobu-x86_64.img --force
```

## Updating Grub2 Bootloader

Grub2 bootloader is used in almost every modern linux distro, you don't need to if you're using LiLo, if you are use [other bootloaders](https://wiki.archlinux.org/title/Category:Boot_loaders).

```bash
# Update Grub2
update-grub

# Incase update-grub command not available
grub-mkconfig -o /boot/grub/grub.cfg
# or
grub2-mkconfig -o /boot/grub/grub.cfg
```