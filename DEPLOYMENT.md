# Deploying Mandala to GitHub Pages

This is a complete static website — no build tools, no Node.js, no dependencies.
Deployment takes about 5 minutes.

---

## Prerequisites

- A GitHub account (free at github.com)
- Git installed on your computer, OR just use the GitHub web interface

---

## Method 1 — GitHub Web Interface (easiest, no Git required)

### Step 1 — Create a repository

1. Go to **github.com** and sign in.
2. Click the **+** icon (top right) → **New repository**.
3. Name it `mandala` (or anything you like).
4. Set it to **Public** (GitHub Pages requires public repos on the free plan).
5. Leave all other options as default. Click **Create repository**.

### Step 2 — Upload the files

1. On your new empty repository page, click **uploading an existing file**.
2. Drag and drop the entire contents of this folder into the upload area:
   - `index.html`
   - `articles.html`
   - `about.html`
   - `contact.html`
   - `css/` folder (with `style.css`, `article.css`)
   - `articles/` folder (with `template.html` and any article files)
3. Add a commit message like `Initial site upload`.
4. Click **Commit changes**.

### Step 3 — Enable GitHub Pages

1. In your repository, go to **Settings** → **Pages** (left sidebar).
2. Under **Source**, select **Deploy from a branch**.
3. Under **Branch**, select `main` and `/ (root)`.
4. Click **Save**.
5. Wait 1–2 minutes. GitHub will show you your live URL:
   `https://YOUR-USERNAME.github.io/mandala/`

That's it. Your site is live. ✓

---

## Method 2 — Git command line

```bash
# 1. Navigate to your project folder
cd path/to/mandala

# 2. Initialise a git repository
git init
git add .
git commit -m "Initial commit"

# 3. Create a repository on GitHub (github.com → New repository)
#    Copy the remote URL they give you, then:
git remote add origin https://github.com/YOUR-USERNAME/mandala.git
git branch -M main
git push -u origin main

# 4. Enable Pages in GitHub Settings → Pages → Branch: main / root
```

---

## Method 3 — Custom domain (optional)

If you own a domain (e.g. `mandalajournal.in`):

1. In GitHub Pages settings, enter your domain under **Custom domain**.
2. Create a file called `CNAME` in the root of your repository containing
   just your domain name:
   ```
   mandalajournal.in
   ```
3. At your domain registrar, add a CNAME DNS record:
   - **Name:** `www`
   - **Value:** `YOUR-USERNAME.github.io`
4. For the apex domain (`mandalajournal.in` without www), add four A records
   pointing to GitHub's IPs:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
5. GitHub will automatically provision an HTTPS certificate (takes up to 24h).

---

## Adding new articles

No CMS or build step required. Just:

1. **Copy** `articles/template.html` to `articles/your-article-slug.html`.
2. **Fill in** all the `[PLACEHOLDER]` values in the new file.
3. **Write** your article content in the `<main class="art-body">` section.
4. **Add a link** to `articles.html`:
   - Open `articles.html` in a text editor.
   - Copy any existing `<li class="article-list-item">` block.
   - Paste it at the top of `<ul id="article-list">`.
   - Update: `data-theme`, tag, title, `href`, abstract, author, date, read time.
5. **Add to the home page** (optional): copy an article card in `index.html`.
6. **Commit and push**:
   ```bash
   git add .
   git commit -m "Add article: your-article-title"
   git push
   ```
   GitHub Pages will update within 1–2 minutes.

---

## Making the contact form work

GitHub Pages is static — it cannot process form submissions server-side.
Use one of these free services:

### Option A — Formspree (recommended)

1. Sign up at **formspree.io** (free tier: 50 submissions/month).
2. Create a new form, copy the form endpoint ID.
3. In `contact.html`, change the `<form>` tag:
   ```html
   <!-- Before -->
   <form action="#" method="POST" ...>

   <!-- After -->
   <form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" ...>
   ```
4. Remove the `onsubmit="handleSubmit(event)"` attribute.
5. Formspree will email you each submission.

### Option B — Netlify Forms (if you switch hosting to Netlify)

Add `netlify` to the `<form>` tag:
```html
<form netlify name="contact" method="POST" action="/thanks.html">
```
Netlify automatically captures submissions.

### Option C — Web3Forms (free, no account needed)

1. Get a free access key at **web3forms.com**.
2. Add a hidden input to your form:
   ```html
   <input type="hidden" name="access_key" value="YOUR-KEY">
   ```
3. Change the form action:
   ```html
   <form action="https://api.web3forms.com/submit" method="POST">
   ```

---

## Keeping Google Fonts working offline (optional)

The site loads fonts from Google Fonts via a CSS `@import`. If you want the
site to work without an internet connection or prefer not to depend on an
external CDN, download the fonts locally:

1. Go to **google-webfonts-helper.herokuapp.com**
2. Search for `DM Serif Display` and `Inter`, download the WOFF2 files.
3. Put them in a `fonts/` folder in your project.
4. Replace the `@import` at the top of `css/style.css` with `@font-face`
   declarations pointing to your local files.

---

## File structure reference

```
mandala/
├── index.html          ← Home page
├── articles.html       ← Article listing
├── about.html          ← About page
├── contact.html        ← Contact & submission
├── DEPLOYMENT.md       ← This file
├── css/
│   ├── style.css       ← Main stylesheet (shared)
│   ├── article.css     ← Article page typography
│   └── partials.html   ← Component reference (not a live page)
└── articles/
    ├── template.html   ← Copy this to create a new article
    ├── informal-tenure.html
    ├── bus-priority.html
    └── [your articles here...]
```

---

## Troubleshooting

**Site not updating after push?**
GitHub Pages can take 2–5 minutes to rebuild. Hard-refresh your browser
(Ctrl+Shift+R / Cmd+Shift+R) to clear the cache.

**Fonts not loading?**
The site requires an internet connection to load Google Fonts. See the
offline fonts section above if you need local fonts.

**CSS not applying?**
Check that file paths in `<link rel="stylesheet">` use the correct relative
path. From article pages (in `/articles/`), the path is `../css/style.css`.
From root pages, it's `css/style.css`.

**Images not showing?**
Use relative paths for images. From an article file, reference images as
`../images/filename.jpg`. Store images in an `/images/` folder at the root.

---

*Mandala is built with plain HTML and CSS — no frameworks, no build tools,
no node_modules. It will work on any static host: GitHub Pages, Netlify,
Cloudflare Pages, Vercel (static), or even a basic web server.*
