### Use your own boot logo

If you want to use your own logo, First install the following tools:

* Debian/Ubuntu

```bash
sudo apt install imagemagick netpbm
```

* Arch/Manjaro

```bash
sudo pacman -S imagemagick netpbm
```

* Fedora

```bash
sudo dnf install ImageMagick netpbm-progs
```

Use ImageMagick to resize the image, adjust it to your console frame buffer screen, i.e. a 1920x1080p monitor can only display 240 columns and 67 rows of the console because 1 column is 8px and 1 row is 16px (8 * 240 = 1920px while 16 * 67 = 1072px) so you need to resize the image to 1920x1072px to fit on a 1920x1080p monitor.

```bash
magick images.png -resize 1920x1072\! images-1920x1072.png
```

Convert to 224-bit color Portable Pixmap and place to `/usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm`

```bash
pngtopnm images-1920x1072.png | ppmquant 224 | pnmnoraw > /usr/src/linux-$kver/drivers/video/logo/logo_linux_clut224.ppm
```

---
