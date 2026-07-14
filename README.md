# AI Multi-Language Tableau Extension
### POC — Claude-powered column header translation

---

## Files in this package

```
multilang.trex   ← Register this in Tableau Desktop/Server
index.html       ← The extension UI (must be hosted over HTTPS)
README.md        ← This file
```

---

## Step 1 — Host index.html over HTTPS

Tableau requires extensions to be served over HTTPS. Easiest free option:

### Option A: Netlify Drop (fastest — 2 minutes)
1. Go to https://app.netlify.com/drop
2. Drag the entire folder onto the page
3. Netlify gives you a URL like: https://amazing-name-123.netlify.app
4. Copy that URL

### Option B: GitHub Pages
1. Push this folder to a GitHub repo
2. Settings → Pages → Deploy from main branch
3. URL will be: https://yourusername.github.io/repo-name

### Option C: Local dev (Tableau Desktop only)
- Run: `npx serve . --ssl` in this folder
- URL will be: https://localhost:3000

---

## Step 2 — Update the .trex file

Open `multilang.trex` and replace this line:

```xml
<url>REPLACE_WITH_YOUR_HOSTED_URL/index.html</url>
```

With your actual URL from Step 1, e.g.:

```xml
<url>https://amazing-name-123.netlify.app/index.html</url>
```

---

## Step 3 — Add the extension to your Tableau dashboard

1. Open your multi-language Tableau workbook
2. In a dashboard, drag an **Extension** object onto the canvas
   (it's in the Objects panel on the left)
3. Click **"Access Local Extensions"** (for dev) or **"Search Extensions"**
4. Browse to and select `multilang.trex`
5. Click **Allow** when Tableau asks for permissions
6. Resize the extension zone to fit your layout

---

## Step 4 — Get a Claude API key

1. Go to https://console.anthropic.com
2. Create an account / sign in
3. API Keys → Create Key
4. Copy the key (starts with `sk-ant-api03-...`)

---

## Step 5 — Use it

1. Paste your API key in the **API Key** field at the top of the extension
2. Select a **worksheet** from the dropdown
   - The extension will load that worksheet's data immediately in English
3. Click **Français** or **Svenska**
4. Click **✨ Translate**
5. Claude translates all column headers in 1–2 seconds
6. Switching back to the same language uses the **cache** (instant)

---

## How it integrates with your existing flag buttons

If your dashboard already has a **[Language] parameter** (named "Language" or "Lang"),
the extension auto-detects it and listens for changes.

When a user clicks your 🇫🇷 flag navigation button and it changes the parameter,
the extension will automatically re-translate to match — no extra clicks needed.

---

## Production security note

For production (not POC), never expose the API key in the frontend.
Instead, proxy it through a small backend:

```
Extension → Your Backend API → Claude API
```

The backend holds the key securely. For POC purposes, the direct call is fine.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Tableau API not detected" | Normal when opening index.html directly in a browser — only works inside Tableau |
| Translation returns garbled JSON | Retry — very rare; Claude occasionally adds extra text |
| HTTP 401 error | API key is invalid or expired |
| HTTP 429 error | Rate limit hit — wait 30 seconds and retry |
| Worksheet not in dropdown | Extension only sees worksheets on the SAME dashboard |
