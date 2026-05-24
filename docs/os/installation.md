# Installing KODE OS

This guide walks you through installing KODE OS on a Raspberry Pi from
scratch. It takes about **fifteen minutes** end-to-end, most of which is
waiting for the SD card to flash.

If you bought a pre-built pebble from KODE NAS, you don't need this page —
your pebble already has KODE OS on it. Skip ahead to [First boot
setup](first-boot.md).

## What you'll need

Before you start, gather these. You probably have most of them already.

- **A Raspberry Pi.** Pi 5 (4 GB or 8 GB) recommended. Pi 4 (4 GB or 8 GB)
  also works. Older models aren't supported.
- **A microSD card.** 16 GB or larger. Class 10 / A1 or better — slow cards
  make everything else feel slow.
- **A USB-C power supply.** The official Raspberry Pi 5 power supply (5 V /
  5 A) is what we recommend. Underpowered supplies cause random reboots that
  are hard to diagnose.
- **An ethernet cable.** Wi-Fi works, but a wired connection is faster and
  more reliable for a home server.
- **An SD card reader.** Built into most laptops; otherwise a cheap USB one
  works fine.
- **Another computer.** Mac, Windows, or Linux — anything that can run
  Raspberry Pi Imager.

!!! info "About the Pi 5"
    KODE OS is developed and tested on the Pi 5. Everything works on the Pi 4
    too, but the Pi 5 is noticeably snappier in the dashboard and faster at
    handling photo backups.

## Installation steps

### 1. Download the latest KODE OS image

Head to the [KODE OS releases page on
GitHub](https://github.com/KodeNAS/kode-os/releases) and download the most
recent `.img.xz` file. It's around 1.5 GB.

You don't need to decompress it — Raspberry Pi Imager handles that for you.

### 2. Install Raspberry Pi Imager

Grab it from the [official Raspberry Pi
website](https://www.raspberrypi.com/software/) and install it like any
other app on your computer.

If you'd rather use a different flashing tool, [balenaEtcher](https://etcher.balena.io/)
works too — see the tabs below.

### 3. Flash KODE OS to your SD card

Insert your microSD card into your computer.

!!! warning "This will erase everything on the SD card"
    Flashing replaces the entire contents of the card. Anything currently on
    it will be gone for good. **Double-check you're picking the right card
    in the imager** — it's easy to pick a USB drive or an external disk by
    mistake.

=== "Raspberry Pi Imager"

    1. Open Raspberry Pi Imager.
    2. Click **Choose device** → pick your Pi model (Pi 5, Pi 4, etc.).
    3. Click **Choose OS** → scroll to the bottom → **Use custom** → select
       the `kode-os-*.img.xz` you downloaded.
    4. Click **Choose storage** → pick your microSD card.
    5. Click **Next**. When asked about OS customisation, choose **No** —
       KODE OS handles its own setup wizard on first boot.
    6. Confirm and wait. Flashing takes 3–7 minutes depending on your card.

=== "balenaEtcher"

    1. Open Etcher.
    2. Click **Flash from file** → select the `kode-os-*.img.xz` you
       downloaded.
    3. Click **Select target** → pick your microSD card.
    4. Click **Flash!** and wait. Etcher verifies the write automatically
       when it finishes.

When the imager says it's safe to remove the card, eject it from your
computer.

### 4. Insert the SD card and power up

1. Slot the microSD card into the underside of your Raspberry Pi.
2. Plug an ethernet cable from the Pi into your router or switch.
3. Plug in the USB-C power supply.

The Pi will boot. The first boot takes 1–2 minutes while KODE OS expands the
filesystem and sets itself up. The green activity LED on the Pi will flicker
the whole time — that's normal.

If you have a pebble with an OLED screen on the front, it will start showing
the pebble's name and IP address once boot is done.

### 5. Find your pebble on the network

You can reach the dashboard in three ways. Try them in order — the first one
that works is the easiest.

=== "By name"

    Open a browser and go to:

    ```
    http://kode.local
    ```

    This works on most home networks. If your browser shows "site not found",
    your network doesn't support mDNS — try the next tab.

=== "By IP address"

    If your pebble has an OLED screen, the IP address is shown there. Open a
    browser and go to:

    ```
    http://<that-ip-address>
    ```

    No OLED? Check your router's admin page — there's usually a list of
    connected devices. Look for one called **kode** or **pebble**.

=== "From another computer"

    On Linux or macOS, you can also try:

    ```bash
    ping kode.local
    ```

    to see the IP address, then visit `http://<that-ip>` in your browser.

### 6. Complete the setup wizard

The first time you open the dashboard, you'll see the **welcome wizard**.
It asks you to:

- Set an admin password.
- Pick a name for your pebble.
- Optionally add family member accounts.
- Pick which apps to install (photos, files, media, etc.).

Take your time. None of it is permanent — you can change anything later from
the **Settings** screen.

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
