# KODE NAS Docs

Source for [docs.kodenas.dev](https://docs.kodenas.dev) — the documentation site for KODE OS and the KODE NAS pebble.

Built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/). Deployed on Cloudflare.

## Local development

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Open `http://127.0.0.1:8000` in your browser.

## Build

```bash
mkdocs build
```

Output goes to `site/`.

## Structure

- `docs/os/` — KODE OS documentation
- `docs/pebble/` — KODE NAS pebble hardware documentation

## Related repos

- [kode-os](https://github.com/KodeNAS/kode-os) — the OS itself
- [kode-os-ui](https://github.com/KodeNAS/kode-os-ui) — the dashboard UI
- [kode-nas-site](https://github.com/KodeNAS/kode-nas-site) — kodenas.dev source

## Contributing

Found a typo or want to improve a page? Open a pull request. Each page is plain Markdown in `docs/`.

## License

The documentation content (everything under `docs/`, including text,
screenshots, and diagrams) is licensed under
[Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/) —
see [LICENSE](LICENSE).

You can share and adapt the material with attribution, for
non-commercial use. The KODE NAS name, logo, "pebble" and "KODE OS" are
trademarks and are not granted by this license.

The KODE OS *operating system* this documentation describes is separately
licensed under Apache 2.0 — see
[KodeNAS/kode-os](https://github.com/KodeNAS/kode-os).
