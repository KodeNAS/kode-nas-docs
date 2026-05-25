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

MIT — see [LICENSE](LICENSE).
