# membrane-site

Website for The Membrane, a fail-closed authorization gate for AI agents.

Live at https://membrane.dojopop.live

The gate itself (Rust code, docs, license) lives in [membrane-hq/the-membrane](https://github.com/membrane-hq/the-membrane).

## Layout

Plain static files, no build step. Serve the repository root.

- `index.html`, `app.js`, `styles.css`: home page
- `for-ip/`: page for IP and licensed-content readers
- `privacy/`, `terms/`: legal pages
- `operator-dashboard/`: simulated operator dashboard demo (sample data)
- `assets/`: icons and share image
