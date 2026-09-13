# DNS cutover — Squarespace → GitHub Pages

Captured 2026-09-12. Tick each step as it's done.

**Primary:** `wisdomsurgery.clinic` serves the GitHub Pages site.
**Secondaries:** `wisdomsurgery.dental`, `wisdomsurgery.com.au` and `wisdomsurgery.au` redirect to it.

## What's true today

| Domain | Registrar | DNS | Points at | Mail |
|---|---|---|---|---|
| `wisdomsurgery.clinic` | Squarespace Domains | `ns-cloud-c*.googledomains.com` | Squarespace `198.185.159.145` | **Microsoft 365** |
| `wisdomsurgery.dental` | Squarespace Domains | `ns-cloud-b*.googledomains.com` | Squarespace (4 A records) | none (`v=spf1 -all`) |
| `wisdomsurgery.com.au` | Tucows/OpenSRS (reg. Anna Raymond) | `ns-cloud-e*.googledomains.com` | Squarespace `198.49.23.144` | none |
| `wisdomsurgery.au` | Tucows/OpenSRS | `ns-cloud-b*.googledomains.com` | Squarespace `198.49.23.145` | none |

All four are managed in **Squarespace Domains**, not GoDaddy. All four serve the same Squarespace site.
Only `.clinic` carries mail — see the next section.

GitHub Pages site: <https://wisdom-surgery.github.io/wisdomsurgery/> — repo `Wisdom-Surgery/wisdomsurgery`,
branch `main`, path `/`, no custom domain set.

Authoritative TTL on every record is **14400s (4 hours)**. There is **no CAA record**, so GitHub can issue a cert.

Every href and src in this repo is relative — zero root-relative `/…` paths — so the site works unchanged
at a domain root. No base-path rewrite needed.

## Do not touch these records

`wisdomsurgery.clinic` carries the practice's Microsoft 365 mail:

```
MX   0 wisdomsurgery-clinic.mail.protection.outlook.com.
TXT  "MS=ms14286809"
TXT  "v=spf1 include:spf.protection.outlook.com -all"
```

Changing A / CNAME records does not affect mail. Deleting or replacing MX or TXT **will break
every practice email address**. Only the A and CNAME records change.

---

# Part 1 — the primary: wisdomsurgery.clinic

## 1. Drop the TTL first (optional, do a day ahead)
Squarespace Domains → `wisdomsurgery.clinic` → DNS. Set the TTL on the apex A records and the
`www` CNAME to **300 seconds**. Wait 4 hours. This makes a rollback take 5 minutes instead of 4 hours.

## 2. Replace the DNS records
Squarespace Domains → `wisdomsurgery.clinic` → DNS.

**Delete** the four Squarespace apex A records (`198.185.159.144/145`, `198.49.23.144/145`)
and the `www` CNAME to `ext-sq.squarespace.com`.

**Add** at the apex (`@`):

```
A     @   185.199.108.153
A     @   185.199.109.153
A     @   185.199.110.153
A     @   185.199.111.153
AAAA  @   2606:50c0:8000::153
AAAA  @   2606:50c0:8001::153
AAAA  @   2606:50c0:8002::153
AAAA  @   2606:50c0:8003::153
```

**Add** for `www`:

```
CNAME www  wisdom-surgery.github.io.
```

## 3. Wait for propagation, then verify
```
dig +short wisdomsurgery.clinic A          # expect the four 185.199.x.153
dig +short www.wisdomsurgery.clinic CNAME  # expect wisdom-surgery.github.io.
```

## 4. Set the custom domain on GitHub
Only after step 3 verifies. Repo → Settings → Pages → Custom domain → `wisdomsurgery.clinic` → Save.
This writes a `CNAME` file to the repo root; pull it down afterwards so local and remote agree.

GitHub then provisions a Let's Encrypt certificate. It can take up to an hour.

## 5. Enforce HTTPS
Once the cert is issued, tick **Enforce HTTPS** in Settings → Pages.

## Rollback
Put the four Squarespace A records and the `www` CNAME back. With a 300s TTL it recovers in minutes.
Remove the custom domain in GitHub Pages settings at the same time, or the github.io URL will keep
redirecting to a domain that serves Squarespace.

---

# Part 2 — the secondaries

Three domains redirect to the primary: **wisdomsurgery.dental**, **wisdomsurgery.com.au**,
**wisdomsurgery.au**. All three are Squarespace domains, all three currently serve the old
Squarespace site, and **none of them carry mail** — no MX records on any of them, so there is no
email risk here. `wisdomsurgery.dental` has a `TXT "v=spf1 -all"` (a "this domain sends no mail"
declaration) which should stay.

GitHub Pages allows **one custom domain per repository**, so each domain needs its own repo.
Each is a single redirect page, set once and never touched again.

| Domain | Redirect repo | Built | On GitHub |
|---|---|---|---|
| `wisdomsurgery.dental` | `Wisdom-Surgery/wisdomsurgery-redirect-dental` | yes | not yet created |
| `wisdomsurgery.com.au` | `Wisdom-Surgery/wisdomsurgery-redirect-com-au` | yes | not yet created |
| `wisdomsurgery.au` | `Wisdom-Surgery/wisdomsurgery-redirect-au` | yes | not yet created |

Local source: `/Volumes/Mac Work/VS Studio Master/wisdomsurgery-redirects/`. Each holds an
`index.html` and an identical `404.html` (meta refresh + JS `location.replace` + `rel=canonical`
+ `noindex, follow`), a `CNAME` naming its domain, and a README. Committed locally, remotes set,
not yet pushed — the repos do not exist on GitHub yet.

Note they cannot be private: Pages only serves from a private repo on a paid org plan, and
Wisdom-Surgery is on the free tier. Each repo contains nothing but a redirect page.

## DNS for each of the three
Squarespace Domains → the domain → DNS. Same shape as Part 1. Delete the Squarespace A records
and the `www` CNAME to `ext-sq.squarespace.com`, then add:

```
A     @    185.199.108.153
A     @    185.199.109.153
A     @    185.199.110.153
A     @    185.199.111.153
CNAME www  wisdom-surgery.github.io.
```

Leave `wisdomsurgery.dental`'s `TXT "v=spf1 -all"` alone.

## Order of operations
1. Create and push the three repos.
2. Enable Pages on each (Settings → Pages → source `main` / `/`). The `CNAME` file sets the domain.
3. Change that domain's DNS in Squarespace.
4. Wait for the cert, then tick Enforce HTTPS.

Do the primary in Part 1 first. A secondary that redirects to a `.clinic` still on Squarespace
just sends people to the old site — harmless, but pointless until the primary has flipped.

**Alternative if a true 301 matters for search:** put the three domains behind Cloudflare's free tier
and use redirect rules — one account instead of three repos, and a real 301 rather than a meta
refresh. It means moving nameservers off Squarespace for each domain. Since all four domains have
been serving byte-identical Squarespace content, there is no separate ranking to preserve and the
difference is small.

---

# Part 3 — after all four are live

## Old Squarespace URLs will 404

The Squarespace site has six indexed pages. The GitHub site is one page plus a few standalone files,
so these break after the cutover:

```
/home  /about  /about-1  /contact  /appointments  /faq-1
```

GitHub Pages cannot do server-side redirects. The fix is a small HTML file at each path with a
meta refresh to the matching section anchor on the new index. **Not yet built.**

## Turn off the Squarespace site

Only once the new site has been live and correct for a few days. Cancel the Squarespace *site*
subscription. Keep the three domain registrations where they are — they're separate from the site plan.
