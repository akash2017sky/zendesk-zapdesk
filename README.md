# Zapdesk — Zendesk Lightning Tips (QR + NWC)

A Zendesk (ZAF v2) sidebar app that lets **end-users tip agents** with Bitcoin Lightning.

- **Wallet-agnostic**: **QR (BOLT11/LNURL-pay)** and **Nostr Wallet Connect (NWC)**.
- **End-user message**: user can add a short “thank you” with their tip.
- **Ticket posting**: app auto-posts that the agent was tipped (public or internal, configurable).

![847b3c6c-7894-4c53-a2b3-486cc8b88b83](https://github.com/user-attachments/assets/9bc94d6b-ebea-44bf-a098-03c274e91c1f)

> Built as a packaged React app (Zendesk React scaffold baseline).  
> No backend required.

---

## Features

- 🧾 **QR / Invoice** — Generate and display **BOLT11** or **LNURL-pay** target + QR.
- 💬 **User message** — free-text input included in the NWC memo and appended to the ticket.
- 📨 **Ticket update** — after a successful tip, Zapdesk posts a message (public/internal).
- ⚙️ **Admin settings** — tip presets, enable/disable NWC/QR, agent address field key, post visibility.

---

## Architecture

- ZAF v2 **iframe app** (React, Vite, Garden), served from `/assets`.
- **Payout target**: agent Lightning Address from a Zendesk user field (configurable).
- **Payment flows**
  - **QR/LNURL-pay**: show QR + copyable string.

---

## Install (dev)

Prereqs: Node 18+, ZCLI (`npm i -g @zendesk/zcli`), Zendesk dev/sandbox.

```bash
git clone https://github.com/KnowAll-AI/zendesk-zapdesk
cd zendesk-zapdesk
npm i
zcli apps:server
```

Open your Zendesk sandbox → Admin → Apps → Manage → Development → Load app from localhost.

## Build & Package

```
npm run build          # emits /assets (iframe.html, app.js, app.css)
zcli apps:package      # produces distributable .zip
```

Upload the zip in Admin Center → Apps and integrations → Zendesk Support apps → Upload app.

## Marketplace Submission

To submit to the Zendesk Marketplace, you'll need:

- **At least 3 screenshots** showing the app in action
  - Recommended: App sidebar view, QR code display, tip confirmation, settings panel
  - Size: 1024x768px or larger
  - Format: PNG or JPG
- **Manifest with `"private": false`** (already configured)
- **No LICENSE.txt files in assets** (webpack configured to exclude)

Add screenshots by placing them in a `screenshots/` folder or as directed by Zendesk's submission process.

## Configure (App settings)

- Tip presets: e.g. 100,1000,10000
- Enable modes:
  - `showQrMode` (QR/LNURL)
- Agent address field key: e.g. `user.custom_fields.lightning_address`
- Fallback address (optional)
- Ticket post visibility: `public` or `internal`
- Branding (optional): title/description

## Usage (end-user flow)

- Open ticket → Zapdesk shows presets and a message box (optional).
- Choose QR (scan/copy) to tip.
  - On success:
  - The app posts to the ticket (public/internal as configured).
- Agent receives funds to their Lightning Address.

## Security

- Iframe sandbox (ZAF v2). No custody; end-user’s wallet makes the payment.
- No WebLN.

## Pre-requisite: Custom Field

- Add custom field
  - go to - https://{subdomain}.zendesk.com/admin/people/configuration/user_fields
  - Click on Add field
  - Enter type - text, name - lightning address, Field key - lightning_address
- go to - https://{subdomain}.zendesk.com/admin/people/team/members
  - In member table click on manage in support
  - On manage in support page Scroll to bottom you will see previously added lightning_address, enter the lightning email here.
- Now go to ticket you can see Zapdesk App on the right sidebar, click on it, It will show the app.


## License

MIT
