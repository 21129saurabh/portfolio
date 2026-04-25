# Deploy Guide — Ask Saurabh Chatbot Proxy

This sets up a secure Vercel proxy so your Anthropic API key never appears
in your HTML. Total time: ~5 minutes.

---

## Folder Structure

```
portfolio-deploy/
├── api/
│   └── chat.js          ← Vercel serverless function (the proxy)
├── public/
│   └── index.html       ← Your portfolio (chatbot points to /api/chat)
├── vercel.json          ← Vercel routing config
├── package.json
└── README.md
```

---

## Step 1 — Get your Anthropic API key

1. Go to https://console.anthropic.com
2. Click "API Keys" → "Create Key"
3. Copy the key — you'll add it to Vercel in Step 3
4. Never paste it into your HTML or commit it to GitHub

---

## Step 2 — Push this folder to GitHub

```bash
cd portfolio-deploy
git init
git add .
git commit -m "initial deploy"
git branch -M main
git remote add origin https://github.com/21129saurabh/YOUR-REPO-NAME.git
git push -u origin main
```

You can name the repo anything — e.g. `portfolio` or `saurabh-portfolio`

---

## Step 3 — Deploy to Vercel

1. Go to https://vercel.com and sign in with GitHub
2. Click "Add New Project" → import the repo you just pushed
3. Vercel will auto-detect the config. Hit "Deploy"
4. Once deployed, go to your project → Settings → Environment Variables
5. Add this variable:

   | Name                  | Value                        |
   |-----------------------|------------------------------|
   | ANTHROPIC_API_KEY     | sk-ant-your-key-here         |

6. Go to Deployments → click the three dots → "Redeploy" so the key takes effect

---

## Step 4 — Update CORS allowed origins

Open `api/chat.js` and update the `allowed` array with your actual Vercel URL:

```js
const allowed = [
  'https://your-project-name.vercel.app',   // ← your Vercel URL
  'https://21129saurabh.github.io',          // ← if also on GitHub Pages
  'http://localhost:5500',
  'http://127.0.0.1:5500',
];
```

Commit and push — Vercel auto-redeploys.

---

## Step 5 — Test it

Visit your Vercel URL → open the chat bubble → ask something.

The flow is:
```
Browser → /api/chat (Vercel) → Anthropic API → back to browser
```

Your API key is only ever on Vercel's server. Never in the browser.

---

## Local Development

Install Vercel CLI:
```bash
npm i -g vercel
```

Run locally:
```bash
cd portfolio-deploy
vercel dev
```

This starts the proxy at http://localhost:3000 and serves your HTML.
The chatbot auto-detects localhost and points to the local proxy.

---

## Updating your portfolio

Just edit `public/index.html` and push to GitHub.
Vercel redeploys automatically within ~30 seconds.

---

## Cost

- Vercel free tier: 100GB bandwidth, unlimited serverless function calls ✅
- Anthropic API: pay per token. Claude Sonnet ~$3/million input tokens.
  For a portfolio chatbot with light traffic this is essentially free.
