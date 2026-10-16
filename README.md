# go.detroitautomationacademy.com — branded short links

Detroit Automation Academy's short-link service. No vendor, no database, no
build step: GitHub Pages serves one tiny static redirect page per short link
from this repository, at $0.

## How it works

- Each short link is a directory named for its slug (e.g. `middle-leg/`)
  containing one `index.html`.
- That page redirects three ways at once, so it works in every browser:
  a `<meta http-equiv="refresh">` to the destination, a
  `window.location.replace(...)` script, and a visible fallback link showing
  the destination for anyone whose browser blocks both.
- Each page carries `noindex` and a canonical link to its destination, so the
  short URLs never compete with the real pages in search results.
- `LINKS.md` is the master manifest: slug → destination → date → note. If this
  repository or the domain ever had to be rebuilt, the manifest reproduces the
  whole service.
- The root page redirects to the Academy home page; unknown paths return the
  branded `404.html`.

## Add a link

1. Create a directory named for the new slug, e.g. `new-slug/`.
2. Copy any existing slug's `index.html` into it and replace the destination
   URL in all four places (canonical, meta refresh, script, fallback link),
   plus the page title.
3. Add a row to `LINKS.md`.
4. Push both files to `main`. GitHub Pages redeploys automatically.

Slugs are lowercase words joined by hyphens, kept short and unambiguous —
they are meant to be spoken aloud and typed from print.

## DNS (one-time setup, at the domain registrar)

The subdomain is pointed at GitHub Pages with a single record:

| Type | Host | Value |
|---|---|---|
| CNAME | `go` | `smit4786.github.io` |

The `CNAME` file in this repository tells GitHub Pages to answer for
`go.detroitautomationacademy.com`. Once the DNS record resolves, GitHub
provisions an HTTPS certificate for the subdomain automatically (Let's
Encrypt); no certificate work is needed here. Until that DNS record exists,
the repository and Pages configuration are complete but the short URLs do
not resolve.

## Exit guarantee

Nothing about this service depends on a third party staying in business or a
free tier staying free. Worst case, the same directory tree can be served
from any static host, and `LINKS.md` documents every mapping.
