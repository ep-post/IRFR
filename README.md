# Incident Reporting & Family Reconnection Portal (IRFR)

Empower People's secure, multi-step reporting portal for missing persons, found persons,
survivor family reconnection, suspected trafficking, forced/cross-region marriage
concerns, children at risk, self-assistance requests, and updates to existing cases.

This is a static site — a single self-contained `index.html` plus one logo asset.
No build step, no framework, no dependencies to install.

## Files

| File          | Purpose                                                              |
|---------------|-----------------------------------------------------------------------|
| `index.html`  | The entire portal — markup, styles, and all form logic in one file.  |
| `logo.png`    | Empower People logo, referenced by the header.                       |

## How it works

- **8 workflows**, chosen via large tap-friendly cards rather than a dropdown, each
  revealing its own set of fields (missing person, found person, survivor reconnection,
  trafficking, forced marriage, child at risk, self-assistance, existing-case update).
- **4-step flow** for every workflow: select → details → reporter & consent → review & submit.
- **Language switcher** using Google's free website Translate widget (no API key needed).
- **Dark/light theme toggle**, respecting the visitor's system preference by default.
- **Submission** is sent to a Google Apps Script Web App endpoint (see below) as JSON,
  including any uploaded files base64-encoded. There is no server of our own — Apps
  Script writes each submission into a linked Google Sheet.
- **Wix embed support** — the page posts its rendered height to `window.parent` so a
  small Velo snippet in a Wix site can auto-resize an embedding iframe. This is inert
  and harmless if the page is used standalone or on GitHub Pages.

## Before you deploy

Open `index.html` and find this line near the top of the `<script>` block:

```js
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/.../exec';
```

Confirm this points to **your own** deployed Google Apps Script Web App, not a
placeholder. Every form submission — including uploaded files — is sent to this URL.
See "Setting up the Apps Script backend" below if you need to create one.

## Deploying with GitHub Pages (free static hosting)

1. Push this folder to a GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Source**, choose the branch (usually `main`) and the root folder (`/`).
4. Save. GitHub gives you a URL like `https://<username>.github.io/<repo-name>/`.
5. Optional — custom subdomain: add a file named `CNAME` (no extension) at the repo
   root containing just your subdomain, e.g.:
   ```
   irfr.empowerpeople.in
   ```
   Then add a `CNAME` DNS record for that subdomain pointing to `<username>.github.io`.

## Setting up the Apps Script backend

1. Create a new Google Sheet, e.g. "Empower People Portal Submissions".
2. **Extensions → Apps Script**, paste in your submission-handling script (a `doPost`
   function that parses the incoming JSON and appends a row / saves attachments).
3. **Deploy → New deployment → type "Web app"**.
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Copy the deployment URL it gives you and paste it into `GOOGLE_SCRIPT_URL` in
   `index.html`.

## Privacy note

This form collects sensitive information — missing-person details, trafficking
suspicions, survivor accounts. If this repository is public, that exposes the **code**
(including the Apps Script endpoint URL and form logic) to anyone, not any submitted
data — submissions themselves only ever go to your Google Sheet. Even so, consider
keeping this repository **private** and granting access only to people who need to
maintain it, and rotate the Apps Script URL if it is ever exposed alongside real
submission data.

## Safety note shown to reporters

The portal displays this note prominently and repeats it after submission: this form
is **not** a substitute for contacting the Police. If someone is in immediate danger,
the nearest Police Station or Dial 112 should be contacted directly.
