# Putting kwotient.io live

> **Status: done — kept for reference.** Stages one to three are complete.
> This repo *is* `kwotient-site`; www.kwotient.io serves from it over HTTPS.
> Do not follow stage one again — it would create a duplicate repo. To change
> the live page, see *Updating the page* at the bottom.

Two stages: get the page onto GitHub Pages, then point GoDaddy at it.
Stage one runs in Claude Code. Stage two is clicking in the GoDaddy dashboard.

---

## Stage one — publish the page

Open Claude Code anywhere and give it this, replacing `YOUR-USERNAME` with your
GitHub username:

> Create a new PUBLIC GitHub repo called `kwotient-site` under YOUR-USERNAME.
> Copy `index.html` and `CNAME` from `~/Downloads/kwotient-site/` into it, commit,
> and push to the `main` branch. Then enable GitHub Pages on that repo, serving
> from the root of `main`, and set the custom domain to `www.kwotient.io`.
> Tell me the `<username>.github.io` address when you're done.

If it prefers explicit commands, these are them:

```bash
mkdir -p ~/Documents/GitHub/kwotient-site
cp ~/Downloads/kwotient-site/index.html ~/Documents/GitHub/kwotient-site/
cp ~/Downloads/kwotient-site/CNAME      ~/Documents/GitHub/kwotient-site/
cd ~/Documents/GitHub/kwotient-site
git init -b main
git add -A
git commit -m "Kwotient holding page"
gh repo create kwotient-site --public --source=. --remote=origin --push
gh api -X POST repos/:owner/kwotient-site/pages -f source[branch]=main -f source[path]=/
```

**This repo is public and that is correct** — it holds a holding page, nothing
else. It is NOT the record repository, which stays private until the October
filing. Do not put the demo in here.

---

## Stage two — point GoDaddy at it

GoDaddy → **My Products** → find kwotient.io → **DNS** → **Manage DNS**.

Delete any existing `A` record on `@` and any `CNAME` on `www` that GoDaddy
parked there, then add these.

**Four A records.** Type `A`, Name `@`, TTL 1 hour, one record per address:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**Four AAAA records.** Type `AAAA`, Name `@`, TTL 1 hour, one per address:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**One CNAME.** Type `CNAME`, Name `www`, Value `YOUR-USERNAME.github.io`,
TTL 1 hour. Note the trailing structure — no `https://`, no slash.

The A and AAAA records make the bare `kwotient.io` work; the CNAME makes
`www.kwotient.io` work. Both end up at the same page.

---

## Stage three — turn on HTTPS

Wait until the DNS has spread. Usually under an hour, occasionally longer —
GoDaddy states up to 48. Check progress at `dnschecker.org` by searching
`kwotient.io` with type `A`; when most locations show the 185.199 addresses,
you are through.

Then in the repo on GitHub: **Settings → Pages**. The custom domain should
already read `www.kwotient.io` from the `CNAME` file. Tick **Enforce HTTPS**.
That box is greyed out until the certificate is issued, which can take another
hour after DNS resolves. It is worth going back for — without it the site
serves over plain HTTP and browsers will say so.

---

## What this costs

Nothing. GitHub Pages is free for public repositories, and you already own the
domain. There is no hosting bill to cancel later.

## Updating the page

Edit `index.html`, commit, push. It is live within a minute or two. No build
step, no framework, one file.
