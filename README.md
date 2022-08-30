# linux-5.19.5-shinobu-x86_64

shinobu-x86_64 is a personal kernel configuration based on the latest stable [kernel](https://kernel.org) release.

## Kernel Compilation

Copy the kernel configuration first before starting compilation.

```bash
# If you need to configure
make menuconfig 

# Kernel compilation
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
cp -iv arch/x86/boot/bzImage /boot/vmlinuz-5.19.5-shinobu-x86_64

# Install System.map
cp -iv System.map /boot/System.map-5.19.5-shinobu-x86_64
```
## Install Kernel Documentation (Optional)

```bash
# Install kernel documentation (optional)
install -d /usr/share/doc/linux-5.19.5-shinobu-x86_64
cp -r Documentation/* /usr/share/doc/linux-5.19.5-shinobu-x86_64
```

## Generate Initramfs (Optional)

```bash
# Generate initramfs (optional)
dracut --kver 5.19.5-shinobu-x86_64 /boot/initramfs-5.19.0-shinobu-x86_64.img --force
```