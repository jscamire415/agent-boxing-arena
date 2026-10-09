# Agent Boxing Arena — public replay

Self-contained static version of the Agent Boxing Arena. Two Agentforce agents
box it out on pre-recorded answers; the winner is saved as the default agent
for the page in `localStorage`. No login, no Salesforce dependency.

Sourced from the Experience Cloud static resource `AgentArenaSite` and
re-vendored for GitHub Pages hosting.

## Contents

```
.
├── index.html            # entire arena (SVG robots, CSS keyframes, replay engine)
├── assets/
│   └── tailwind.js       # @tailwindcss/browser@4.2.2 runtime (vendored, ~272KB)
├── .nojekyll             # skip Jekyll processing
└── .github/workflows/
    └── deploy.yml        # GitHub Pages Actions deploy on push to main
```

Everything else — favicon, SVG robots, CSS, recorded answers, scoring — is
inline in `index.html`. The only external asset is `assets/tailwind.js`,
loaded with a relative path so it works at any repo subpath.

## Deploy to GitHub Pages (recommended)

1. Create a new empty repo (public or private — Pages is free on either since
   GH free Pages for private repos).
2. Push this directory:
   ```sh
   git init -b main
   git add .
   git commit -m "feat: public replay of Agent Boxing Arena"
   git remote add origin git@github.com:<you>/<repo>.git
   git push -u origin main
   ```
3. In the repo on github.com → **Settings → Pages**:
   - **Source**: *GitHub Actions*
4. The workflow in `.github/workflows/deploy.yml` publishes on every push to
   `main`. Live URL will be `https://<you>.github.io/<repo>/`.

If your GitHub account is SSO-locked through Salesforce and the push fails,
either (a) authorize an SSH key / PAT for the SSO org in your account
settings, or (b) use one of the alternative hosts below.

## Alternative hosts (no changes needed)

Everything resolves via relative paths — drop the folder on any static host:

- **Netlify** — `netlify deploy --dir . --prod`
- **Vercel** — `vercel --prod`
- **S3 + CloudFront** — upload the directory; set `index.html` as the default
  root object
- **Heroku / Cloud Run** — serve with any static Nginx container
- **Salesforce Experience Cloud static resource** — zip and upload as a
  `StaticResource` with `Cache Control: Public` (original hosting mode)

## Refreshing recorded bouts

Answers in `index.html` are embedded as data. To refresh them, re-record
agents in Loveboat (or the source org) and regenerate the file via the
authoring tooling in the parent `agent-arena-site/` kit.
