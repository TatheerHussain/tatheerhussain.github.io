# 🎓 Academic Portfolio — Dynamic Single-Page Website

A modern, dark-themed academic portfolio website built as a **single HTML file** with a built-in **admin panel** for live content management. No frameworks to install, no build tools, no backend servers — just one file deployed on GitHub Pages with a custom domain.

> **Live Demo:** [https://tatheerhussain.info](https://tatheerhussain.info)

---

## ✨ Features

### For Visitors
- **Responsive dark-mode design** with gradient accents, particle animations, and glassmorphism
- **Home** — Hero section, bio, research interests, latest news, education, experience, important reads
- **Publications** — Journal articles and conference papers with venue tags, plus a **Current Projects** sub-tab
- **Gallery** — Year-wise photo albums + embedded YouTube video player
- **Blog** — Grid of blog posts with summaries and external links
- **CV** — Toggleable sections (Summary, Education, Experience, Publications) + custom sections
- **Social footer** — Icon links on every page (Email, GitHub, Medium, ORCID, Scholar, LinkedIn, Twitter/X)

### For the Owner (Admin)
- **Hidden admin access** — No visible login button. Access via:
  - Clicking the **logo 5 times** rapidly, or
  - Pressing **Ctrl + Shift + A**
- **Full CMS** — Add, edit, delete, and reorder content across all sections
- **Publish to GitHub** — One-click publishing that commits a `data.json` file to your repo via the GitHub API
- **Dynamic data** — Site loads content from `data.json` on GitHub, so changes are permanent and visible to everyone
- **Secure token handling** — GitHub token is entered at login and stored in memory only; never written to disk or code
- **CV builder** — Toggle built-in sections on/off, add custom sections, reorder with arrow controls
- **Toast notifications** — Visual feedback for publish success/failure
- **Unsaved changes banner** — Persistent reminder bar when you have unpublished edits

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| UI Framework | React 18 (loaded from CDN) |
| Transpiler | Babel Standalone (loaded from CDN) |
| Styling | Inline CSS-in-JS |
| Font | Google Fonts (Inter) |
| Data Storage | GitHub API (`data.json` in repo) |
| Hosting | GitHub Pages |
| Domain | Custom domain via DNS |

**Zero dependencies. Zero build step. One HTML file.**

---

## 🚀 How to Replicate

### Step 1 — Create Your Repository

1. Create a new GitHub repo named `<your-username>.github.io`
2. Make sure it's **public** (required for free GitHub Pages)

### Step 2 — Set Up the Files

Your repo needs just **2 files**:

```
your-username.github.io/
├── index.html    ← The portfolio (copy from this repo)
├── CNAME         ← (Optional) Your custom domain
└── data.json     ← Auto-created on first publish
```

### Step 3 — Customize the Defaults

Open `index.html` and find the `const DEFAULTS = {` block near the top. Replace the default data with your own:

```javascript
const DEFAULTS = {
  name: "Your Name",
  title: "Your Title",
  affiliation: "Your University / Lab",
  location: "Your City, Country",
  email: "you@example.com",
  orcid: "",
  github: "your-github-username",
  medium: "",
  scholar: "",
  linkedin: "",
  twitter: "",
  profilePhoto: "",
  bio: "Your bio here...",
  researchInterests: ["Topic 1", "Topic 2"],
  // ... edit all sections
};
```

### Step 4 — Update GitHub Config

Find these lines and replace with your GitHub username and repo name:

```javascript
const GH_OWNER = "YourGitHubUsername";
const GH_REPO  = "your-username.github.io";
```

### Step 5 — Change the Admin Password

Find and replace:

```javascript
const PASS = "admin123";
```

⚠️ **Change this to a strong password before deploying.**

### Step 6 — Create a GitHub Fine-Grained Personal Access Token

1. Go to [GitHub Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens](https://github.com/settings/tokens?type=beta)
2. Click **Generate new token**
3. Set:
   - **Token name:** `portfolio-site` (or anything)
   - **Expiration:** 90 days or custom
   - **Repository access:** Select **"Only select repositories"** → pick your `.github.io` repo
   - **Permissions:** Set **Contents** to **Read and write** (everything else: No access)
4. Click **Generate token**
5. **Copy the token** — you'll need it when logging in as admin

### Step 7 — Deploy

1. Push `index.html` (and `CNAME` if using a custom domain) to your repo
2. Go to **Settings → Pages → Source** → select `main` branch, `/ (root)` → Save
3. Your site will be live at `https://<your-username>.github.io` within 1–2 minutes

### Step 8 — (Optional) Custom Domain

To use a custom domain like `yourname.com`:

1. Add a `CNAME` file to your repo containing just: `yourname.com`
2. At your domain registrar, add these DNS records:

| Type | Name | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | your-username.github.io |

3. Verify domain at **GitHub → Settings → Pages → Add a domain** (profile-level, not repo-level)
4. Enable **Enforce HTTPS** in repo Settings → Pages

---

## 📝 Usage Guide

### Logging In as Admin

1. Go to your site
2. **Click the logo (top-left) 5 times quickly** — or press **Ctrl + Shift + A**
3. Enter your **admin password** and **GitHub token**
4. You're now in admin mode (green "● ADMIN" badge appears)

### Editing Content

- Every section shows **Edit** and **Delete** buttons in admin mode
- Each section has an **"+ Add"** button to create new items
- The CV page has **toggle controls** and **reorder arrows**

### Publishing

1. Make your edits (a purple "You have unpublished changes" banner appears)
2. Click **Publish** (in the nav bar or the banner)
3. Changes are committed to `data.json` in your repo
4. Everyone sees the updates immediately (after GitHub Pages cache refreshes, ~1 min)

### Token Security

Your GitHub token is:
- ✅ Entered at login, stored **in memory only**
- ✅ Never saved to localStorage, cookies, or code
- ✅ Scoped to one repo with contents-only permission
- ✅ Cleared when you close the tab
- ❌ Never hardcoded in the HTML file

---

## 📁 Project Structure

```
index.html          → Entire application (React + CSS + data)
data.json           → Site content (auto-created/updated via admin panel)
CNAME               → Custom domain config (optional)
README.md           → This file
```

### How Data Flows

```
┌─────────────┐     loads data.json      ┌──────────────┐
│   Visitor    │ ◄──────────────────────  │  GitHub Repo │
│  (browser)   │                          │  data.json   │
└─────────────┘                          └──────────────┘
                                               ▲
┌─────────────┐     commits via API       │
│    Admin     │ ────────────────────────► │
│  (browser)   │   (GitHub token in RAM)
└─────────────┘
```

1. **On page load:** Fetches `data.json` from the public GitHub API (no token needed)
2. **Falls back to `DEFAULTS`** if `data.json` doesn't exist yet
3. **On publish:** Commits updated `data.json` to the repo using the GitHub Contents API

---

## 🎨 Customization

### Changing Colors

The color scheme uses Indigo/Violet throughout. Search and replace these values in `index.html`:

| Token | Current | What it affects |
|-------|---------|----------------|
| `#6366f1` | Indigo-500 | Primary accent |
| `#8b5cf6` | Violet-500 | Gradient secondary |
| `#a5b4fc` | Indigo-300 | Text accent |
| `#818cf8` | Indigo-400 | Links, labels |
| `#0a0a0f` | Near-black | Background |

### Adding New Pages

1. Create a new page component function (e.g., `TeachingPage`)
2. Add it to the `navItems` array
3. Add the render condition in the `<main>` block

### Adding New Editable Fields

1. Add the field to `DEFAULTS`
2. Add edit UI using the `openEdit()` pattern used throughout

---

## ⚠️ Important Notes

- **Public repo required** for free GitHub Pages. Upgrade to GitHub Pro ($4/mo) for private repo support.
- **Token expiration:** Regenerate your GitHub token before it expires (you set the duration when creating it).
- **Cache delay:** After publishing, GitHub Pages may take 1–2 minutes to reflect changes due to CDN caching.
- **Single-file architecture:** Everything is in `index.html`. This is intentional for simplicity and zero-config deployment. For larger sites, consider splitting into separate files.
- **Babel in-browser transpilation** adds ~200ms to initial load. For production-critical performance, pre-compile the JSX.

---

## 📄 License

MIT License — Free to use, modify, and distribute.

---

## 🙏 Credits

Built with [React](https://reactjs.org/), [Google Fonts](https://fonts.google.com/), and the [GitHub API](https://docs.github.com/en/rest).

Template created for academic researchers who want a modern portfolio without the complexity of static site generators.