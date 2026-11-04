# Galer V LLC website

The company website at https://galer-v.com, hosted with GitHub Pages under the Galer-V organization.

The static HTML, CSS, and SVG files in this repository are published directly from the root of `main`. No build step or backend is required. `.nojekyll` disables Jekyll processing; `CNAME` retains the custom domain. Changes pushed to `main` deploy through GitHub Pages.

Preview locally: `python3 -m http.server 4318`, then open http://localhost:4318.

DNS remains at GoDaddy. Apex A records point to GitHub Pages; `www` is a CNAME to `galer-v.github.io`. Email records are managed separately and should be preserved.
