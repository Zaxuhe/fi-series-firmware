# epjitsu firmware

Firmware files for Epson/Fujitsu flatbed scanners using the SANE `epjitsu` backend — specifically the **FI-60F**, **FI-65F**, and **FI-70F** models.

These files are required by SANE to talk to the scanner over USB. They are not redistributed by most Linux distros, so drop them in place manually.

## Files

| File | Scanner |
| --- | --- |
| `60f_0A06.nal` | Epson FI-60F |
| `65f_0A01.nal` | Epson FI-65F |
| `70f_0A02.nal` | Epson FI-70F |

## Installation

### Debian / Ubuntu

```bash
git clone https://github.com/zaxuhe/fi-series-firmware.git
sudo mkdir -p /usr/share/sane/epjitsu
sudo cp fi-series-firmware/*.nal /usr/share/sane/epjitsu/
sudo apt install sane libsane-dev
```

Log out and log back in. Test with `scanimage -L` or [Simple Scan](https://gitlab.gnome.org/GNOME/simple-scan).

### Arch Linux

```bash
sudo pacman -S sane
sudo mkdir -p /usr/share/sane/epjitsu
sudo cp *.nal /usr/share/sane/epjitsu/
```

If your device is not auto-detected, add it to `/etc/sane.d/epjitsu.conf`:

```
usb 0x04c5 0x11a2   # FI-65F — check lsusb for your ID
```

## Usage

List detected scanners:

```bash
scanimage -L
```

Basic scan:

```bash
scanimage --device epjitsu:libusb:001:021 --format=png --mode=Color --resolution=300 > scan.png
```

With brightness/contrast tweaks (helps for faded cards):

```bash
scanimage --device epjitsu:libusb:001:021 \
  --format=png --mode=Color --resolution=300 \
  --brightness 50 --contrast 20 > scan.png
```

## Requires patched SANE

If scans still crash or triplicate on FI-60F/FI-65F, your `sane-backends` predates MR !887. Either build `sane-backends` from source with the patch, or wait for a release that includes it.

## License

Firmware files are the property of their respective vendors and are mirrored here for Linux users who own the hardware. Everything else is MIT.
