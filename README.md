# Rocco website

Public pages for the Rocco iPhone app, served by GitHub Pages from the public repo `kXrock/rocco-website`:

| page | file | address |
|---|---|---|
| Home | `index.html` | https://rocco.keremkaya.dev/ |
| How it works | `how-it-works.html` | https://rocco.keremkaya.dev/how-it-works.html |
| Pricing | `pricing.html` | https://rocco.keremkaya.dev/pricing.html |
| FAQ | `faq.html` | https://rocco.keremkaya.dev/faq.html |
| About | `about.html` | https://rocco.keremkaya.dev/about.html |
| Guides | `guides/index.html` | https://rocco.keremkaya.dev/guides/ |
| Guide: daily cleaning routine | `guides/daily-cleaning-routine.html` | https://rocco.keremkaya.dev/guides/daily-cleaning-routine.html |
| Guide: weekly cleaning schedule | `guides/weekly-cleaning-schedule.html` | https://rocco.keremkaya.dev/guides/weekly-cleaning-schedule.html |
| Guide: split chores fairly | `guides/split-chores-fairly.html` | https://rocco.keremkaya.dev/guides/split-chores-fairly.html |
| Guide: motivation to clean | `guides/motivation-to-clean.html` | https://rocco.keremkaya.dev/guides/motivation-to-clean.html |
| Compare | `compare/index.html` | https://rocco.keremkaya.dev/compare/ |
| Compare: Rocco vs Sweepy | `compare/rocco-vs-sweepy.html` | https://rocco.keremkaya.dev/compare/rocco-vs-sweepy.html |
| Compare: Rocco vs Tody | `compare/rocco-vs-tody.html` | https://rocco.keremkaya.dev/compare/rocco-vs-tody.html |
| Compare: Rocco vs FlyLady | `compare/rocco-vs-flylady.html` | https://rocco.keremkaya.dev/compare/rocco-vs-flylady.html |
| Compare: Sweepy alternatives | `compare/sweepy-alternatives.html` | https://rocco.keremkaya.dev/compare/sweepy-alternatives.html |
| Privacy Policy | `privacy.html` | https://rocco.keremkaya.dev/privacy.html |
| Support | `support.html` | https://rocco.keremkaya.dev/support.html |
| App Store redirect (noindex) | `go/index.html` | https://rocco.keremkaya.dev/go/ |
| Not found (noindex) | `404.html` | served by GitHub Pages for any missing path |

Terms of Use: Apple's standard EULA (https://www.apple.com/legal/internet-services/itunes/dev/stdeula/).

The source lives in the private app repo (`kXrock/rocco`, folder `site/`). Edit it there, then publish:

```bash
python3 site/tools/check.py          # must print PASS
bash site/publish.sh "Privacy: mention X"
```

`publish.sh` does not publish `tools/` and, after a successful push, pings IndexNow (`tools/indexnow.sh`); a failed ping
does not fail the publish.

The same addresses are used in the app (`mobile/src/shared/config/links.ts`), in App Store Connect
(Privacy Policy URL, Support URL, Marketing URL) and in Google Auth Platform (Branding).
When the privacy policy changes, update its "Effective" date, its `<lastmod>` in `sitemap.xml`, its `dateModified`
in the page's JSON-LD and the App Privacy answers in App Store Connect.

## SEO / GEO

Goal: search engines and AI answer engines (ChatGPT, Claude, Perplexity, Gemini, Copilot) can crawl, understand and
quote the site. All pages are plain HTML that works without JavaScript.

| file | what it is for |
|---|---|
| `<head>` of every page | title, description, canonical, Open Graph / Twitter card, `apple-itunes-app` banner, icons, manifest |
| JSON-LD in every page | structured data. The full app entity (`#app`, with features, languages, version and the 4 subscription offers), `#website` and `#maker` live on the home page; other pages point at them by `@id` |
| `robots.txt` | allows every crawler, names the AI crawlers explicitly, blocks only `go/`, points to the sitemap |
| `sitemap.xml` | every indexable page with `<lastmod>` (not `go/`, not `404.html`) |
| `llms.txt` | short map of the site for AI assistants (llmstxt.org format) |
| `llms-full.txt` | the complete fact sheet about Rocco in one markdown file, written to be quoted |
| `site.webmanifest`, `apple-touch-icon.png` | icons and app link for browsers and iOS |
| `0f8a17b5224b9176849829932d3f29bd.txt` | IndexNow key file (must stay published; the name and content are the key) |
| `404.html` | friendly not-found page, `noindex`, absolute `/` paths because it is served at any depth |
| `tools/check.py` | local checks, not published |
| `tools/indexnow.sh` | tells Bing and other IndexNow engines that every sitemap URL changed |
| `tools/set-domain.sh` | moves the site to a new address (see below) |
| `tools/faq_jsonld.py` | after editing answers in `faq.html`: `python3 site/tools/faq_jsonld.py site/faq.html` rebuilds its FAQPage data from the visible text |
| `tools/guides_jsonld.py` | after editing a guide: `python3 site/tools/guides_jsonld.py` rebuilds the JSON-LD of every page in `guides/` from the visible text |

Run the checks before every publish:

```bash
python3 site/tools/check.py
```

It checks every page (one title, description length, canonical, Open Graph, JSON-LD parses, internal links and images
exist, no em or en dashes, no `index.html` links, a warning when "game" appears next to "Rocco is"), that `sitemap.xml`
lists exactly the indexable pages, that `llms.txt` links exist, and that no subscription price other than $6.99, $29.99,
$11.99 and $49.99 appears (competitor prices are allowed only in `compare/`). Every page in `compare/` must exist and
carry the trademark and facts note.

### When a price, feature or version changes

The same facts are written in several places. Update all of them, then run `tools/check.py`:

- `index.html`: the JSON-LD `#app` node (`offers`, `featureList`, `softwareVersion`, `dateModified`, `inLanguage`) and the visible FAQ plus its `FAQPage` JSON-LD (they must match word for word).
- `pricing.html`: the visible table and its `Offer` JSON-LD.
- `faq.html`: the visible answers and its `FAQPage` JSON-LD.
- `how-it-works.html` and `about.html` when a feature or the maker details change.
- `llms.txt` (key facts) and `llms-full.txt` (everything; also its "Last updated" date).
- `sitemap.xml`: `<lastmod>` of every page you changed.
- `compare/*` (and the "How Rocco compares" section of `llms-full.txt`): Rocco's facts there must match too. The facts
  about Sweepy, Tody and FlyLady live only in these pages and must be re-checked against their App Store pages before
  each edit (Sweepy https://apps.apple.com/us/app/id1498897320, Tody https://apps.apple.com/us/app/id595339588,
  FlyLady https://apps.apple.com/us/app/id1115477521); then update the "checked on" date in the note on every compare page.

### Address

The site is served from its own subdomain, `https://rocco.keremkaya.dev` (GitHub Pages custom domain: the `CNAME` file in
this folder, and a DNS-only CNAME record `rocco` -> `kxrock.github.io` in Cloudflare). The old addresses under
`https://kxrock.github.io/rocco-website/` redirect here, so the links in the app, App Store Connect and Google Auth
Platform keep working. Keep the DNS record "DNS only" (grey cloud): a proxied record would put Cloudflare's bot rules
between AI crawlers and the site. `robots.txt` and `llms.txt` now sit at the host root, where crawlers look for them.

To move again: `bash site/tools/set-domain.sh https://new.example.com` swaps the address in every site file (pages,
JSON-LD, sitemap, llms files, tools); then change `CNAME` and the DNS record.

### Off-site setup (state on 2026-11-15)

- [x] Google Search Console: Domain property `keremkaya.dev` (covers `rocco.` and the portfolio), verified by a TXT
      record at the zone apex in Cloudflare (do not delete it). `https://rocco.keremkaya.dev/sitemap.xml` submitted,
      indexing of the home page requested.
- [x] Bing Webmaster Tools: site `https://rocco.keremkaya.dev/`, verified by the CNAME record
      `69d3b8f0758dfbc2ea54e9909f0d858c.rocco` -> `verify.bing.com` in Cloudflare (do not delete it). Sitemap submitted.
      Bing's index also feeds Copilot and several AI search tools; its "AI Performance" report shows AI citations.
- [x] GitHub: Enforce HTTPS is on for `kXrock/rocco-website`; `keremkaya.dev` is a verified Pages domain of the
      account (TXT record `_github-pages-challenge-kXrock` in Cloudflare, do not delete it), so no other GitHub
      account can claim a subdomain of it.
- [ ] App Store Connect: set the Marketing URL to `https://rocco.keremkaya.dev/`.
- [ ] Link the website from the Instagram, TikTok and Facebook profiles.
