# linux-5.19.0-shinobu-x86_64

shinobu-x86_64 is a personal kernel configuration based on mainline [kernel](https://kernel.org) release.

## Kernel Compilation

Copy the kernel configuration first before starting compilation

```bash
# If you need to configure
make menuconfig 

# Kernel compilation
make -j$(nproc)
```

## Kernel Installation

There are two methods to install the kernel

* Method 1

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
make -j$(nproc) install

# Install kernel documentation
install -d /usr/share/doc/linux-5.19.0-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-5.19.0-shinobu-x86_64
```

* Method 2 (manually)

```bash
# Install kernel modules
make -j$(nproc) modules_install

# Install kernel
cp -iv arch/x86/boot/bzImage /boot/vmlinuz-5.19.0-shinobu-x86_64

# Install system.map
cp -iv System.map /boot/System.map-5.19.0-shinobu-x86_64

# Install kernel documentation
install -d /usr/share/doc/linux-5.19.0-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-5.19.0-shinobu-x86_64
```

## Generate Initramfs (optional)

```bash
# Generate initramfs (optional)
dracut --kver 5.19.0-shinobu-x86_64 /boot/initramfs-5.19.0-shinobu-x86_64.img --force
```