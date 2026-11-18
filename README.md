# membrane-site

Website for The Membrane, a fail-closed authorization gate for AI agents.

Live at https://themembrane.org

The gate itself (Rust code, docs, license) lives in [membrane-hq/the-membrane](https://github.com/membrane-hq/the-membrane).

## Layout

Plain static files, no build step. Serve the repository root.

- `index.html`, `app.js`, `styles.css`: home page
- `for-ip/`: page for IP and licensed-content readers
- `privacy/`, `terms/`: legal pages
- `operator-dashboard/`: simulated operator dashboard demo (sample data)
- `assets/`: icons and share image

## Agent-readable documentation

- `llms.txt`: concise documentation index linking to source Markdown.
- `agent-guide.md`: local evaluation and deployment boundaries.
- `_headers`: Cloudflare Pages content types for those static files.

These files help documentation discovery and evaluation. They do not imply that
all agents read this convention or that any third-party index has accepted it.
The evaluator referenced by `agent-guide.md` lives in the Rust repository; ship
that change first so the documented command exists before this page goes live.
