# Galer V company website — setup and operations

## Ownership and layout

- Company repository: https://github.com/Galer-V/galer-v.com (public, owned by the Galer-V organization).
- Local checkout: `/Users/phil/coding/galer-v/company-site/`.
- Host: GitHub Pages, branch `main`, folder `/`.
- Main domain: `galer-v.com`. GitHub Pages redirects `www.galer-v.com` to the main domain.
- Registrar and authoritative DNS: GoDaddy (`ns51.domaincontrol.com`, `ns52.domaincontrol.com`).
- There is no AWS service, database, backend, or ChatGPT service required to run the site.

`index.html`, `styles.css`, and `assets/` are the complete website. `.nojekyll` disables Jekyll so these files are served directly. `CNAME` contains `galer-v.com`; preserve it when editing or replacing the site. The Google Fonts stylesheet is optional: local fallback fonts are defined in CSS.

## DNS

The following records were saved in GoDaddy on November 4, 2026 and verified against the authoritative nameserver. They replace the registrar parking destination and the temporary ChatGPT destination.

| Type | Host | Value |
| --- | --- | --- |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | galer-v.github.io |

The existing Google Workspace MX, SPF, DKIM, DMARC, Google verification, nameservers, and unrelated records were not changed. Do not replace the entire DNS zone when changing web hosting. No ChatGPT verification TXT records were added.

Reference: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site

## Publishing updates

1. Edit the static files in this checkout. Preview with `python3 -m http.server 4318` and open `http://localhost:4318`.
2. Check the page, local asset paths, email links, Notyfi link, and narrow-screen layout.
3. Commit and push to `main` (or merge a reviewed branch). GitHub Pages publishes automatically. No build dependencies or deployment credentials are stored in the repo.
4. Check the repository's Actions / Pages deployment and visit both company domain names.

Useful commands, using the GitHub CLI signed in as an organization administrator:

```sh
git status --short
git push origin main
gh api repos/Galer-V/galer-v.com/pages
gh api repos/Galer-V/galer-v.com/pages/builds/latest
gh api repos/Galer-V/galer-v.com/pages/health
```

## HTTPS and verification

GitHub provisions and renews the certificate. On November 4, 2026, both domains passed GitHub's DNS health checks. GitHub approved a certificate covering both domains and HTTPS enforcement was enabled. Verified: `https://galer-v.com` returned HTTP 200 from `Server: GitHub.com`, and `https://www.galer-v.com` returned HTTP 301 to `https://galer-v.com/`, both with valid certificate verification.

HTTPS enforcement is already enabled. To restore it if settings are reset, use **Enforce HTTPS** in repository Settings → Pages, or run:

```sh
gh api --method PUT repos/Galer-V/galer-v.com/pages -F https_enforced=true
curl -I https://galer-v.com
curl -I https://www.galer-v.com
```

Both HTTPS requests must pass certificate verification. The `www` request should redirect to the main domain. Do not bypass certificate errors. Apple review readiness still depends on HTTPS working and Apple's own enrollment review.

For DNS troubleshooting:

```sh
dig +short A galer-v.com @ns51.domaincontrol.com
dig +short CNAME www.galer-v.com @ns51.domaincontrol.com
```

Expected answers are the four addresses and CNAME above. Avoid extra A/AAAA records pointing at old hosts, registrar forwarding, or wildcard records. If GitHub reports DNS valid but HTTPS is unavailable, check Settings → Pages for certificate issuance status; issuance is asynchronous.

During the initial setup, certificate issuance stalled until the custom domain was removed and immediately re-added in GitHub Pages settings, following GitHub's documented troubleshooting procedure. GitHub then approved the certificate for both names. This settings change created automatic `CNAME` commits; they have been pulled into the local checkout. Reference: https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https

## Recovery and history

Revert a bad content commit with `git revert <commit>` and push. Preserve the `CNAME` file and GitHub Pages settings. To move hosts, deploy and configure the new host before changing only the website DNS records.

The initial website was briefly deployed to ChatGPT Sites before the owner requested company hosting. Its company-domain bindings were removed, and its duplicate preview was made owner-only with explicit permission. The old local checkout in `/Users/phil/coding/galer-v/website/` is superseded and must not be used for future deployments. This repository is the canonical source.

Company name and registered-office address were taken from the LLC formation documents in `/Users/phil/Dropbox/Galer-V/Formation/`. Only the intended public company details are included in this repository; no formation documents, account credentials, or tax identifiers were copied.
