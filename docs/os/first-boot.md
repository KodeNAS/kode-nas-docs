# First Boot Setup

The first time you power on a freshly-flashed pebble, KODE OS runs
through a one-shot setup sequence. After it finishes, every future
boot is a normal ~30-second boot to the dashboard.

## What happens, step by step

1. **Filesystem expansion.** The image is ~2.8 GB but your SD card
   is probably 16 GB or more. The first-boot service runs
   `raspi-config nonint do_expand_rootfs` so the root filesystem
   fills the whole card.

2. **Wait for network.** Before installing anything, the service
   pings the internet (specifically `github.com`, the next thing
   it needs). If you forgot to plug in the Ethernet cable — or
   Wi-Fi hasn't connected yet — the OLED shows
   **"WAITING FOR NETWORK / Plug in Ethernet"** with a live countdown.
   Plug in the cable and setup auto-resumes within 10 seconds. The
   service waits up to 5 minutes before giving up and asking for a
   reboot.

3. **CasaOS bootstrap.** Downloads the CasaOS runtime (Docker
   convenience installer, then the CasaOS gateway + services), drops
   them into `/usr/bin/casaos-*`, enables the systemd units. Takes
   about 2 minutes on a decent network.

4. **Wizard token generation.** A 32-hex-char random token gets
   written to `/opt/kode-os/.wizard-token` (root-only) and
   `/var/lib/casaos/www/.wizard-token` (so the UI router can
   validate the URL).

5. **MOTD + OLED + dashboard wakeup.** The wizard URL goes into
   `/etc/motd` (visible if you plug in a monitor), into the OLED
   status display, and is reachable as `http://pebble.local/`.

6. **Self-disable.** The `.firstboot-pending` marker file is
   removed, so the systemd unit's `ConditionPathExists=` makes
   subsequent boots skip the work entirely.

## What you'll do

Open `http://pebble.local/` in any browser on the same network. The
router auto-fetches the wizard token and redirects you to the actual
wizard URL (`http://pebble.local/#/wizard/<token>`) — you don't have
to type the token yourself.

The wizard asks for:

- An admin password.
- The pebble's display name (the hostname stays `pebble` for mDNS;
  this is just a friendly label).
- Optional family members.
- Which apps to install — Immich for photos, Jellyfin for media,
  File Browser for files, Pi-hole as a network ad blocker, Home
  Assistant for smart home.
- A dashboard layout — pick one of six pre-made templates or build
  your own.

Each app you pick goes through its own walkthrough — opens the app,
creates an account in it, connects mobile clients, finishes with a
"you're done" screen. The whole sequence is meant to take about 5
minutes.

## Why SSH is off

KODE OS ships with SSH disabled by default. The wizard creates an
admin account for the *dashboard* — not for the underlying Linux
shell. This is the security posture for v0.2: dashboard-only access,
no SSH foot-guns by default.

A built-in terminal + log viewer in the Settings panel is planned for
a future release. Until then, if you genuinely need shell access on
the device, see [Troubleshooting → "I want SSH access"](troubleshooting.md#i-want-ssh-access).

## If setup fails

The first-boot service is idempotent. If anything dies mid-setup —
network drops, transient GitHub blip, anything — the
`.firstboot-pending` marker stays in place and the service retries
on the next boot.

If you see **"NO NETWORK / Plug in Ethernet / then reboot"** on the
OLED: plug in the cable, reboot, setup tries again from scratch.

If you see **"SETUP FAILED / Install error — check logs"**: that's
not a network problem. The install hit an actual error and needs a
human to look at the logs. See
[Troubleshooting](troubleshooting.md#setup-failed-on-the-oled-after-first-boot).
