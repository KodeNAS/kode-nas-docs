# Troubleshooting

Common things that go wrong on first-boot or shortly after, and what
to do about each.

If your issue isn't here, the next-best paths are:

- The OLED status display (if you have one wired up) usually narrates
  the immediate failure mode.
- `journalctl -u kode-firstboot -n 100 --no-pager` from a console
  shows everything the first-boot service did.
- GitHub Issues: <https://github.com/KodeNAS/kode-os/issues>

## I can't find `pebble.local` on my network

mDNS (`pebble.local` resolution) isn't reliable on every home
network. If `http://pebble.local/` times out from your laptop, try
finding the IP directly:

1. **Router admin page.** Most home routers have a "connected
   devices" list. Look for a hostname like `pebble` and note its IP.
2. **`avahi-resolve`** (Linux/macOS):
    ```bash
    avahi-resolve -n pebble.local
    # or:
    getent hosts pebble.local
    ```
3. **`nmap` ping sweep:**
    ```bash
    nmap -sn 192.168.1.0/24 | grep -B 2 pebble
    ```

Once you have the IP, open `http://<that-ip>/` in your browser — the
router auto-redirect to the wizard works the same as `pebble.local`.

If you have an OLED wired up, the second screen in the normal
rotation always shows the current IP — no laptop required.

## The OLED is blank

The OLED daemon is hardware-gated — it doesn't run if no SH1122 is
wired to the GPIO header. If you DO have one wired and it's still
dark, check:

1. **SPI is enabled.** The first-boot service writes
   `dtparam=spi=on` to `/boot/firmware/config.txt`. Verify:
    ```bash
    grep dtparam=spi /boot/firmware/config.txt
    ```
    Expected: `dtparam=spi=on` (uncommented). If missing, edit the
    file and reboot.
2. **The device node exists:**
    ```bash
    ls /dev/spidev0.0
    ```
    If it doesn't, SPI isn't loaded — re-check step 1.
3. **The daemon is running:**
    ```bash
    sudo systemctl status kode-nas-display
    ```
    If it's in a crash loop, the journal usually has the reason:
    ```bash
    sudo journalctl -u kode-nas-display -n 30 --no-pager
    ```
4. **Wiring.** The daemon assumes DC = GPIO 24, RST = GPIO 25, plus
   the standard SPI0 CLK/MOSI/CS pins. Double-check against the
   wiring guide in [OLED Display](oled.md).

A wiring or hardware fault never blocks boot — the rest of the
system runs normally even when the OLED is dark.

## I want SSH access

KODE OS v0.2.0-alpha ships with SSH **disabled by default**. The
wizard creates an admin account for the dashboard only; there's no
usable Linux shell login.

The reasoning: the `kode` user account is `passwd -l` locked so
password auth fails everywhere, and shipping with SSH off means
default credentials can never be a problem. A built-in terminal +
log viewer inside the Settings panel is on the roadmap.

If you genuinely need shell access *now* (debugging, custom
software, etc.), the supported path is the `--debug-ssh` build:

1. Clone the [kode-os repo](https://github.com/KodeNAS/kode-os) on
   your laptop.
2. `cd image-build && ./build.sh --debug-ssh` — this builds an
   image variant with SSH enabled and your `~/.ssh/id_ed25519.pub`
   baked in.
3. Flash that image to the SD card.
4. After first-boot, `ssh kode@pebble.local` works with pubkey auth.
   Password auth stays off (the `kode` account is still locked).

The `--debug-ssh` image is suffixed `-debug` in its filename so
there's no risk of confusing it with a release image.

## I lost my wizard URL / token

The token is regenerated on every successful first-boot run. If you
haven't completed the wizard yet, you can get a fresh one without
losing anything:

```bash
# On the pebble's console (HDMI + USB keyboard, or via --debug-ssh):
sudo systemctl stop casaos-user-service casaos-gateway casaos
sudo rm -f /opt/kode-os/.wizard-token /var/lib/casaos/www/.wizard-token

TOKEN=$(openssl rand -hex 16)
sudo bash -c "echo $TOKEN > /opt/kode-os/.wizard-token && chmod 600 /opt/kode-os/.wizard-token"
sudo bash -c "echo $TOKEN > /var/lib/casaos/www/.wizard-token && chmod 644 /var/lib/casaos/www/.wizard-token"

sudo systemctl start casaos-user-service casaos-gateway casaos

IP=$(hostname -I | awk '{print $1}')
echo "New wizard URL: http://${IP}/#/wizard/${TOKEN}"
```

If you've already completed the wizard and forgot your admin
password, see ["I lost my admin password"](#i-lost-my-admin-password)
below.

## "SETUP FAILED" on the OLED after first boot

The first-boot service distinguishes two failure modes:

- **"NO NETWORK / Plug in Ethernet / then reboot"** — no internet was
  reachable within the 5-minute wait window. Plug in the cable, reboot,
  setup tries again.
- **"SETUP FAILED / Install error — check logs"** — the network was
  fine but `install.sh` itself errored. Needs a human.

For the second case, get the logs:

```bash
# Via --debug-ssh image, or HDMI + keyboard:
sudo journalctl -u kode-firstboot -n 100 --no-pager
sudo tail -200 /var/log/kode-firstboot.log
```

The most common cause is a transient GitHub mirror blip during the
CasaOS tarball download — rebooting often clears it because the
`.firstboot-pending` marker survives the failure and the service
retries.

If it keeps failing across reboots, open an issue with the log
output: <https://github.com/KodeNAS/kode-os/issues>.

## I lost my admin password

There's no built-in password-reset for the wizard's admin account
yet. Two paths:

**Fastest — reflash.** Wipes everything except whatever you've
backed up. Quick if you don't have apps configured yet.

**Keep `/DATA` — reset just the user database.** Requires shell
access (`--debug-ssh` image or HDMI + keyboard):

```bash
sudo systemctl stop casaos-user-service casaos-gateway casaos
sudo rm /var/lib/casaos/db/user.db
sudo rm -f /opt/kode-os/.wizard-token /var/lib/casaos/www/.wizard-token
sudo systemctl start casaos-user-service casaos-gateway casaos
```

Then open `http://pebble.local/` — the welcome wizard runs fresh
because no admin exists. Re-create the account with whatever
password you want. Your `/DATA` contents, app configs, and any
files survive.

A proper "reset password" CLI is on the v0.3 roadmap.
