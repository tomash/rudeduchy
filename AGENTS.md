# AGENTS.md

## Cursor Cloud specific instructions

### Product

Single static **Jekyll 4** blog (“Rude Duchy”) — Polish whisky/spirits content. No backend, database, Docker, or `package.json`. See `README.md` for theme origin (Galileo).

### Runtime

- **Ruby 3.2.2** is pinned in `.tool-versions`; use [mise](https://mise.jdx.dev/) (`~/.local/bin/mise`).
- Activate in an interactive shell: `eval "$(mise activate bash)"` (already appended to `~/.bashrc` on the dev VM).
- Non-interactive commands: `mise exec -- <command>` from the repo root.

### Dependencies

```bash
cd /workspace
mise install
mise exec -- bundle install
```

Bundler is locked to **2.4.10** (`Gemfile.lock`). Gems install under the mise-managed Ruby.

**Native extension gotcha:** Ubuntu’s default `/usr/bin/c++` may point to **clang**, which breaks building the `eventmachine` gem (Jekyll’s live-reload dependency). If `bundle install` fails with `iostream file not found`, run:

```bash
sudo update-alternatives --set c++ /usr/bin/g++
```

Then re-run `bundle install`. System packages needed once per VM: `build-essential`, `libyaml-dev`, `libreadline-dev`, `zlib1g-dev`, `libssl-dev`, `g++`.

### Run / build

| Task | Command |
|------|---------|
| Dev server (watch) | `mise exec -- bundle exec jekyll serve -w --host 0.0.0.0 --port 4000` |
| Production build | `mise exec -- bundle exec jekyll build` → `_site/` |

Restart Jekyll after editing `_config.yml`. Default URL: **http://localhost:4000**.

### Lint / test

No project-defined lint or test suite. Validation is `jekyll build` (and manual browsing via `jekyll serve`).

### Services

Only **Jekyll** must run for local development. Optional externals: Google Fonts CDN, Google Analytics scripts in `_includes/javascripts.html` (not required locally).
