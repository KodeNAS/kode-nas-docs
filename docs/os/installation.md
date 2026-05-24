# Installing KODE OS

This guide walks you through installing KODE OS on a Raspberry Pi 5 from
scratch. It takes about **twenty minutes** end-to-end, plus a longer
hands-off wait while the installer pulls Docker, CasaOS, and the KODE OS
dashboard onto the pebble.

If you bought a pre-built pebble from KODE NAS, you don't need this page —
your pebble already has KODE OS on it. Skip ahead to [First boot
setup](first-boot.md).

!!! warning "Alpha software"
    KODE OS is currently **v0.1.0-alpha**. APIs, defaults, and the install
    path will change. Only run it on hardware you can reflash.

## What you'll need

Before you start, gather these. You probably have most of them already.

- **A Raspberry Pi 5 (4 GB or 8 GB).** This is the only model we test
  against. Pi 4 may work but isn't supported.
- **A microSD card.** 64 GB or larger. Class 10 / A1 or better — slow cards
  make everything feel slow. An M.2 NVMe via the Pi 5 HAT is a great upgrade
  if you have one.
- **A USB-C power supply.** The official Raspberry Pi 5 power supply (5 V /
  5 A) is what we recommend. Underpowered supplies cause random reboots that
  are hard to diagnose.
- **An ethernet cable.** Wi-Fi works, but a wired connection is faster and
  more reliable for a home server.
- **An SD card reader.** Built into most laptops; otherwise a cheap USB one
  works fine.
- **Another computer.** Mac, Windows, or Linux — anything that can run
  Raspberry Pi Imager and open an SSH session.

## How the install works

KODE OS isn't a single image you flash to an SD card. It's an installer that
runs on top of a fresh **Raspberry Pi OS Lite (64-bit, Bookworm)** install,
and adds:

1. Docker.
2. The upstream [CasaOS](https://github.com/IceWhaleTech/CasaOS) runtime.
3. The KODE OS dashboard, built and overlaid onto the CasaOS web root.
4. The OLED display daemon (only if you have the OLED accessory).

So the order of operations is: **flash Pi OS Lite → SSH in → run the
installer → open the dashboard.**

## Installation steps

### 1. Flash Raspberry Pi OS Lite to your SD card

Insert your microSD card into your computer and open **Raspberry Pi Imager**
(grab it from the [Raspberry Pi
website](https://www.raspberrypi.com/software/) if you don't have it).

!!! warning "This will erase everything on the SD card"
    Flashing replaces the entire contents of the card. Anything currently on
    it will be gone for good. **Double-check you're picking the right card
    in the imager** — it's easy to pick a USB drive or an external disk by
    mistake.

=== "Raspberry Pi Imager (recommended)"

    1. Open Raspberry Pi Imager.
    2. **Choose device** → **Raspberry Pi 5**.
    3. **Choose OS** → **Raspberry Pi OS (other)** → **Raspberry Pi OS Lite
       (64-bit)**.
    4. **Choose storage** → your microSD card.
    5. **Next** → when asked about OS customisation, click **Edit
       settings**. This step is important — set:
        - **Hostname:** `pebble`
        - **Username:** `kode`
        - **Password:** something memorable; you'll use it once.
        - **Wireless LAN:** only if you're not using ethernet.
        - **Services tab:** turn on **Enable SSH** → **Use password
          authentication** (or paste your public key, if you have one).
    6. **Save** → **Yes** → confirm the erase warning → wait. Flashing
       takes 3–7 minutes depending on your card.

=== "balenaEtcher"

    Etcher can flash the image, but it can't pre-configure SSH, the
    hostname, or the user. You'll have to do that by hand after the first
    boot — either by attaching a monitor and keyboard to the Pi, or by
    mounting the SD card again and creating empty `ssh` and `userconf.txt`
    files in the boot partition. Use Raspberry Pi Imager if you can.

    1. Download the latest **Raspberry Pi OS Lite (64-bit)** image from the
       [official Raspberry Pi OS page](https://www.raspberrypi.com/software/operating-systems/).
    2. Open Etcher → **Flash from file** → pick the downloaded image.
    3. **Select target** → your microSD card.
    4. **Flash!** and wait. Etcher verifies the write automatically when
       it finishes.

When the imager says it's safe to remove the card, eject it from your
computer.

### 2. Boot the Pi

1. Slot the microSD card into the underside of your Raspberry Pi.
2. Plug an ethernet cable from the Pi into your router or switch.
3. Plug in the USB-C power supply.

The Pi will boot. The first boot takes 1–2 minutes while Pi OS expands the
filesystem and applies the imager settings.

### 3. SSH into the Pi

From your other computer, open a terminal and connect:

```bash
ssh kode@pebble.local
```

Type the password you set in the imager. If `pebble.local` doesn't resolve
(some routers don't support mDNS), find the Pi's IP address from your
router's admin page and SSH to that instead:

```bash
ssh kode@<the-ip-address>
```

!!! info "First-time host key prompt"
    SSH will ask whether to trust the host key the first time. Type **yes**
    and hit enter.

### 4. Run the KODE OS installer

Once you're SSHed in, clone the repo and run the installer:

```bash
git clone https://github.com/KodeNAS/kode-os.git
cd kode-os
sudo ./scripts/install.sh
```

The installer runs through seven phases and prints what it's doing the
whole way:

1. Checks you're on a Raspberry Pi with a supported Raspberry Pi OS
   release.
2. Installs Docker.
3. Installs the upstream CasaOS runtime.
4. Installs Node 18 and pnpm.
5. Clones, builds, and overlays the KODE OS dashboard onto the CasaOS web
   root.
6. Installs the OLED display daemon — only if you have the OLED
   accessory (auto-detected).
7. Prints the dashboard URL.

This step takes **10–20 minutes** depending on your network speed. Most of
the wait is Docker and CasaOS being pulled. You can step away.

??? note "Optional installer flags"
    The installer accepts a few flags for less common cases:

    - `--no-oled` — skip the OLED daemon even if the hardware is detected.
    - `--skip-casaos` — install only the KODE OS layer, on a Pi that
      already has CasaOS.
    - `--version REF` — install a specific git ref (tag, branch, or commit)
      of the dashboard rather than the latest.
    - `--uninstall` — remove KODE OS, the OLED daemon, and CasaOS. Your
      `/DATA` folder is left untouched.

### 5. Open the dashboard

When the installer finishes, it prints a URL. Open it in any browser on the
same network — it'll look like:

```
http://pebble.local
```

If that doesn't resolve, use the Pi's IP address (same one you used for
SSH). The first request can take a few seconds while CasaOS warms up.

!!! info "HTTPS is optional"
    The dashboard runs over HTTP by default. If you'd rather have HTTPS with
    a per-pebble local certificate authority, run
    `sudo ./scripts/setup-pebble-https.sh` after the main install — it
    installs Caddy with `tls internal`.

### 6. Run the welcome wizard

The first time you open the dashboard, you'll see the **welcome wizard**.
It walks you through:

- Setting an admin password for the dashboard.
- Naming your pebble.
- Adding family member accounts (optional).
- Picking which apps to install — photos, files, media, ad blocker, smart
  home.
- Choosing a dashboard layout.

Take your time. None of it is permanent — you can change anything later
from the **Settings** screen.

The wizard is covered in detail on the [First boot setup](first-boot.md)
page.

## Next steps

You're done installing. From here:

- [First boot setup](first-boot.md) — what the welcome wizard asks for, and
  why.
- [File sharing (Samba)](samba.md) — connect to your pebble from your Mac,
  Windows PC, or phone.
- [OLED display](oled.md) — if you have the screen accessory, customise what
  it shows.
- [Troubleshooting](troubleshooting.md) — if anything looks wrong.
