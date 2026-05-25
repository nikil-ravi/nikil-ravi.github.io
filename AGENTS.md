# AGENTS.md

## Cursor Cloud specific instructions

This is a Hugo static site (personal blog) using the PaperMod theme.

### Prerequisites

- **Hugo extended** (v0.148.1) — installed via `.deb` from GitHub releases
- **Go** (1.25.1) — required for Hugo module resolution (`go.mod`)
- **Git submodules** — PaperMod theme lives at `themes/PaperMod`

### Key commands

| Action | Command |
|--------|---------|
| Dev server | `hugo server --bind 0.0.0.0 --port 1313 --buildDrafts` |
| Production build | `hugo --gc --minify` |
| Lint (pre-commit) | `pre-commit run --all-files` |

### Notes

- The site uses Hugo modules **and** a git submodule for the PaperMod theme. Both `git submodule update --init --recursive` and `hugo mod get` are needed on first setup.
- `hugo server` builds draft content when `--buildDrafts` is passed; the production build (`hugo --gc --minify`) does not include drafts.
- Pre-commit hooks fix trailing whitespace, end-of-file issues, YAML validation, and large file checks. Many existing files trigger these hooks — this is pre-existing, not a regression.
- No database, Docker, or backend services are needed.
