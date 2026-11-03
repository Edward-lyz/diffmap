# Edward's Diffmap

Live viewer: <https://edward-lyz.github.io/diffmap/>.

This fork deploys the original MIT-licensed Diffmap viewer to GitHub Pages.
It preserves the upstream diagrams, source links, diff panel, themes and local comments.
The npm publishing job runs only in the upstream repository.

## Share a report

Write a normal Diffmap Markdown report outside the product repository. Include a
`changes` fence containing `staged` to snapshot the exact index for a forthcoming
commit, or `working` to review all current changes, including untracked files.

From the repository being reviewed, with GitHub CLI authenticated:

```sh
DIFFMAP_ORIGIN='https://edward-lyz.github.io/diffmap/?path=' \
  npx @tanishqkancharla/diffmap@0.9.2 share /absolute/path/to/report.md
```

The original CLI uploads the Markdown, changes and any referenced source files to
an unlisted GitHub Gist. Anyone with its URL can read it. This public deployment
is intended for public code; do not upload private code or credentials.

Put the returned URL in the commit message:

```text
Fix the concrete behavior

Diffmap: https://edward-lyz.github.io/diffmap/?path=/g/<gist-id>
```

Each commit gets a fresh snapshot. Keep published Gists unchanged so existing
commit links remain meaningful. The independent Gist ID avoids trying to include
a commit's own SHA in its message. Reports live outside the product source tree.

## Routes and deployment

Pages serves a project at `/diffmap/` and cannot rewrite arbitrary SPA paths.
The viewer therefore uses `?path=/g/<gist-id>` or
`?path=/owner/repo/commit/<sha>/path/to/report.md`. These URLs load and refresh
with HTTP 200; heading anchors remain available for the table of contents.

Push `main` to run `.github/workflows/pages.yml`. It uses Node 24, `npm ci`, the
existing typecheck and `vite build --base /diffmap/`, then the official Pages
artifact and deploy actions. Enable **Settings → Pages → GitHub Actions** once.

For a local preview:

```sh
npm ci
npm run build -- --base /diffmap/
npm run preview:gist -- --base /diffmap/
```

Upstream source and documentation: <https://github.com/tanishqkancharla/diffmap>.
