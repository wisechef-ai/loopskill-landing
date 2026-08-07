# loopskill-landing — the apex redirector

`loopskill.io` (GitHub Pages, branch `master`, CNAME in this repo) serves
**nothing but redirects to `https://app.loopskill.io`**, plus the two files that
have to stay at the apex because published URLs point at them.

## Why there is no landing page here any more

Until 2026-08-07 this repo held a full second storefront. It told a different
story from the app — the retired "open registry of runnable agent artifacts"
positioning, a pricing ladder that had been replaced, and nav links to `/docs`,
`/pricing` and `/llms.txt` that returned 404 because those pages only ever
existed on `app.loopskill.io`. Two storefronts drifted for weeks and the apex —
the hostname people actually type — was the stale one.

**`app.loopskill.io` is canonical.** Storefront copy has exactly one source:
[`wisechef-ai/loopskill-portal`](https://github.com/wisechef-ai/loopskill-portal).
The two cannot drift again, because there is no longer a second copy to drift.

## The rule for editing this repo

If you are writing a headline, a price, a feature list, or any sentence a
customer would read as a claim — **stop**. It belongs in the portal. This repo
holds redirects and two legacy files, nothing else. A marketing word appearing
here is the beginning of the exact split that was just removed.

## What is in here

| File | Why |
|---|---|
| `index.html` | Apex root → `app.loopskill.io/`. Meta-refresh + `rel=canonical` + JS. |
| `404.html` | Every other apex path → the same path on `app.loopskill.io`, query and hash preserved. |
| `CNAME` | Binds the Pages site to `loopskill.io`. Deleting it takes the domain down. |
| `robots.txt` | Crawling allowed (crawlers must see the redirect); sitemap points at the app. |
| `install` | `curl -fsSL loopskill.io/install \| sh` is published in the wild. Must keep serving. |
| `og.png`, `og.svg` | Social cards already shared against apex URLs. Cheap to keep, breaks previews to remove. |

`sitemap.xml` was deleted: it advertised `loopskill.io/` as an indexable
destination, which is the opposite of what this repo now does.

## The known ceiling: these are not 301s

GitHub Pages serves static files and cannot emit an HTTP redirect. `index.html`
is a meta-refresh; `404.html` is JS, and Pages returns **HTTP 404** with it no
matter what it contains. Both work for humans; neither is a proper 301 for
crawlers.

The durable fix is to point the apex at the Caddy host that already serves
`app.loopskill.io` and issue real 301s there. The vhost is written and waiting in
the portal repo at `ops/Caddyfile`; the DNS change and the cutover order are
written up in the portal PR that introduced this redirector. Until that lands,
this repo is the interim — and it is already strictly better than serving a
second, contradictory storefront.
