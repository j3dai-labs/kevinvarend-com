# kevinvarend.com

Personal site of Kevin Varend — Co-Founder &amp; Managing Director, J3D.AI Labs. Static HTML, deployed on Vercel from this GitHub repo.

---

## Stack

- **Hosting**: Vercel (free Hobby tier is sufficient for this site)
- **Source control**: GitHub
- **Build**: none — pure HTML / CSS / inline JS, served from `/public`
- **DNS**: Namecheap (or your registrar of choice)
- **Domain**: kevinvarend.com

---

## Local development

No build step. Open `public/index.html` directly in a browser, or run any static server:

```bash
# Python 3
cd public && python -m http.server 8000

# Node
npx serve public
```

Visit http://localhost:8000

---

## Deployment

**Automatic.** Every push to `main` triggers a Vercel deploy. Pull requests get preview URLs.

To deploy manually from the command line:

```bash
npm i -g vercel
vercel --prod
```

---

## Repository structure

```
kevinvarend-com/
├── public/
│   └── index.html          # The site
├── .github/
│   └── workflows/
│       └── lighthouse.yml  # Optional: Lighthouse CI on PRs
├── vercel.json             # Vercel config (headers, redirects, caching)
├── .gitignore
├── LICENSE
└── README.md
```

---

## Migration runbook — from current host to GitHub + Vercel

This is the safe, zero-downtime path. Total active time: ~30 minutes. Total elapsed: 24–48 hours (DNS propagation).

### Step 0 — Identify your current host

Open a terminal and run:

```bash
dig kevinvarend.com NS +short
```

If you see `liquidweb.com` → you are on **LiquidWeb managed hosting**, the WordPress install lives there.
If you see `wordpress.com` or `wpengine.com` → you are on a hosted WordPress SaaS.
If something else → make a note, the principle below still applies.

Either way, the hosting is separate from your **domain registration**. Look up where you bought the domain (probably an email receipt from Namecheap, GoDaddy, or wherever) — that's where you control DNS.

### Step 1 — Back up your existing site

Even though we are replacing it, take a backup before touching anything.

**If WordPress (LiquidWeb or otherwise):**
1. Log into `kevinvarend.com/wp-admin`
2. Tools → Export → All Content → Download Export File (XML)
3. Save the XML somewhere durable (Google Drive)
4. Optional: download a full backup via your hosting control panel

This XML contains all posts, pages, and media. You don't need it for the new site, but keep it as insurance.

### Step 2 — Create the GitHub repository

1. Go to https://github.com/new
2. Repo name: `kevinvarend-com` (or whatever you like — just pick something)
3. Visibility: **Private** is fine — Vercel reads private repos
4. Initialize: **without** README (we have one)
5. Click "Create repository"

Then on your local machine:

```bash
# Unzip kevinvarend-com.zip somewhere first
cd kevinvarend-com

git init
git add .
git commit -m "Initial commit — kevinvarend.com static site"
git branch -M main
git remote add origin git@github.com:YOUR_USERNAME/kevinvarend-com.git
git push -u origin main
```

If you don't have SSH set up, swap the remote URL for the HTTPS one GitHub gives you and authenticate with a personal access token.

### Step 3 — Connect to Vercel

1. Go to https://vercel.com/signup and sign in **with GitHub** (this auto-authorises Vercel to read your repos)
2. From the Vercel dashboard, click **Add New → Project**
3. Find your `kevinvarend-com` repo, click **Import**
4. **Framework Preset**: Other (it auto-detects "Other")
5. **Root Directory**: leave as repo root
6. **Build Command**: leave empty
7. **Output Directory**: `public`
8. Click **Deploy**

After ~30 seconds you'll have a live URL like `kevinvarend-com-xyz.vercel.app`. Open it. This is your site running on Vercel — already faster than your current host, on global CDN, free SSL.

**Do not change DNS yet.** Confirm the Vercel preview URL works perfectly first. If anything looks wrong, fix it in `public/index.html`, push to GitHub, and Vercel auto-redeploys in seconds.

### Step 4 — Add your custom domain to Vercel

In the Vercel project:

1. Settings → **Domains**
2. Add `kevinvarend.com`
3. Add `www.kevinvarend.com`
4. Vercel will show you the DNS records you need. **Copy these.**

You'll get either:
- An **A record** pointing the apex (`@`) to `76.76.21.21`, plus
- A **CNAME** pointing `www` to `cname.vercel-dns.com`

Or alternatively, Vercel may suggest its **nameservers** — `ns1.vercel-dns.com` and `ns2.vercel-dns.com` — for full DNS management.

I recommend the **A + CNAME** approach (keeps your DNS at your registrar where you have full control) over delegating nameservers to Vercel. Either works.

### Step 5 — Update DNS at your registrar

Log into wherever you bought the domain. The DNS panel will be different on each registrar but the actions are the same:

1. **Delete** the existing A records pointing to LiquidWeb / WordPress.com
2. **Delete** any existing CNAME for `www`
3. **Add** the new records Vercel gave you in Step 4
4. **Keep** any MX records (these are for email — do NOT touch unless you want to break your email)
5. Save

If your registrar is Namecheap specifically:
- Domain List → Manage → Advanced DNS
- Add Record → A Record → Host: `@` → Value: `76.76.21.21` → TTL: Automatic
- Add Record → CNAME Record → Host: `www` → Value: `cname.vercel-dns.com.` → TTL: Automatic

### Step 6 — Wait, then verify

DNS propagation typically takes 15 minutes – 4 hours, occasionally up to 48. Monitor with:

```bash
dig kevinvarend.com +short
# Should eventually return 76.76.21.21
```

Or use https://dnschecker.org/ — paste your domain, watch the green checks roll across the world map.

Once propagation completes:
1. Visit https://kevinvarend.com — should show the new site
2. Vercel auto-issues a Let's Encrypt SSL certificate within minutes (you'll see "Valid Configuration" with a green check in Vercel → Domains)
3. Both `kevinvarend.com` and `www.kevinvarend.com` should work, with HTTPS

### Step 7 — Cancel old hosting

**Only after Step 6 confirms the new site works.** Wait at least 7 days as a buffer.

- If LiquidWeb: log in, find the kevinvarend.com hosting plan, cancel it. Confirm any final billing.
- If WordPress.com paid plan: same idea — Settings → Plan → Cancel.

Make sure you're cancelling the **hosting** plan, not the **domain registration** if they happen to be in the same place. The domain itself is yours and should keep auto-renewing.

### Optional Step 8 — Move domain to Namecheap

You said you want to consolidate at Namecheap. This is independent from the hosting migration above — your site is already on Vercel either way.

To move the registration:
1. At your current registrar: unlock the domain, request the EPP / auth code
2. At Namecheap: Domains → Transfer In → enter `kevinvarend.com` and the auth code → pay (~€10, includes a year renewal)
3. Approve the transfer email
4. Wait 5–7 days for the transfer to complete

Your DNS records carry over automatically with the transfer in most cases, but verify after transfer that the Vercel records are still there.

---

## What this setup gets you

- **Speed**: global CDN, sub-100ms first byte from anywhere
- **Cost**: free (Vercel Hobby + Namecheap domain only)
- **Workflow**: edit in any text editor, `git push`, live in seconds
- **Previews**: every PR gets a unique preview URL — useful for sharing draft updates with collaborators
- **SSL**: automatic, auto-renewing, no Cloudflare needed
- **Headers**: HSTS, X-Frame-Options, content-type sniffing protection — all set in `vercel.json`
- **Caching**: aggressive on static assets, no-cache on HTML — get the latest content immediately on each deploy

---

## Editing the site

Open `public/index.html` in any text editor. The whole site is one self-contained file with inline CSS and a tiny bit of JS for tab interactions and scroll reveals.

When you're done:

```bash
git add public/index.html
git commit -m "Update [whatever you changed]"
git push
```

Vercel deploys within ~30 seconds.

---

## License

The HTML, CSS and content of this repository are © Kevin Varend. The repo itself is private.

---

## Contact

Questions on the deploy: check Vercel docs at https://vercel.com/docs.
Questions on DNS: check Namecheap support at https://www.namecheap.com/support/.
