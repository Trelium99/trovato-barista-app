# ☕ Barista Bell — Customer Call Kiosk

A lightweight, static single-page kiosk app for cafés. Customers tap a button to notify the barista via Telegram. No backend, no subscriptions, no servers — just a static HTML file.

---

## How It Works

1. The iPad sits on the counter displaying the kiosk
2. A customer taps **Call Barista**
3. A Telegram message is sent instantly to the barista's phone
4. The barista heads over — no need to be at the counter 24/7

---

## Stack

- Pure HTML / CSS / JS — zero dependencies, zero build step
- [Telegram Bot API](https://core.telegram.org/bots/api) for push notifications
- Hosted as a **free static site** on [DigitalOcean App Platform](https://www.digitalocean.com/products/app-platform)
- Credentials stored in `localStorage` — local to the iPad only, never sent to any server

---

## Deployment — DigitalOcean App Platform

### 1. Push to GitHub

Create a new repo and push `index.html` to it:

```bash
git init
git add index.html
git commit -m "init"
git remote add origin https://github.com/YOUR_USERNAME/barista-kiosk.git
git push -u origin main
```

### 2. Create App on DigitalOcean

1. Go to [cloud.digitalocean.com](https://cloud.digitalocean.com) → **Apps** → **Create App**
2. Connect your GitHub account and select the `barista-kiosk` repo
3. DigitalOcean will auto-detect it as a **Static Site**
4. Set:
   - **Output Directory:** `/`
   - **Index Document:** `index.html`
5. Click **Deploy**

Your app will be live at a URL like:
```
https://barista-kiosk-xxxx.ondigitalocean.app
```

**Cost: Free** — DigitalOcean App Platform static sites are free.

---

## Telegram Bot Setup

### Step 1 — Create a Bot

1. Open Telegram and search for **@BotFather**
2. Send `/newbot`
3. Follow the prompts — give it a name and username
4. BotFather will reply with your **Bot Token**:
   ```
   7482910345:AAFxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```
   Save this.

### Step 2 — Get Your Chat ID

1. Search for your new bot in Telegram and tap **Start**
2. Send it any message (e.g. "hello")
3. Open this URL in your browser (replace `<TOKEN>` with your bot token):
   ```
   https://api.telegram.org/bot<TOKEN>/getUpdates
   ```
4. Find the `"chat"` object in the JSON response:
   ```json
   "chat": { "id": 123456789, ... }
   ```
   That number is your **Chat ID**.

### Step 3 — Configure the Kiosk on the iPad

1. Open the kiosk URL in Safari on the iPad
2. Tap **⚙ Setup** (bottom right)
3. Enter the PIN (default: **1234**)
4. Paste your **Bot Token** and **Chat ID**
5. Optionally customise the notification message
6. Tap **Save & Activate**

The credentials are saved to the iPad's `localStorage` — they stay on the device and are never transmitted anywhere except directly to the Telegram API.

---

## PIN Protection

The setup screen is protected by a 4-digit PIN to prevent customers accessing configuration.

- **Default PIN:** `1234`
- **To change it:** Open Setup → enter current PIN → scroll to **Change PIN** at the bottom → enter new 4-digit PIN → tap **Update PIN**

> The PIN is stored in `localStorage` on the device. It is separate from the Telegram credentials.

---

## iPad Kiosk Mode

To lock the iPad to only show the kiosk (recommended for production):

1. Go to **Settings → Accessibility → Guided Access**
2. Enable **Guided Access** and set a passcode
3. Open Safari and navigate to your kiosk URL
4. Triple-press the **Side/Home button** to start Guided Access
5. Tap **Start**

The iPad will now be locked to that one page. Triple-press the side button and enter the Guided Access passcode to exit.

---

## Notification Cooldown

After a customer taps the bell, the button is disabled for **8 seconds** to prevent notification spam. This is hardcoded in `index.html` and can be adjusted by changing the `8000` value (milliseconds) in the `callBarista` function.

---

## File Structure

```
barista-kiosk/
└── index.html    # The entire app — one file
```

---

## Security Notes

- Credentials are stored in `localStorage` which is **local to the device and browser** — a customer visiting the same URL on their own device will see an unconfigured kiosk
- The only risk is someone with **physical access to the iPad** opening browser DevTools — in a managed kiosk environment (Guided Access enabled) this is not possible
- The Telegram Bot Token is visible in browser network requests — this is unavoidable with a static frontend; the token only has permission to send messages to your specific chat

---

## Customisation

| What | Where in `index.html` |
|---|---|
| Welcome heading | `<h1>Need a <em>Barista?</em></h1>` |
| Notification cooldown | `8000` in `setTimeout` inside `callBarista()` |
| Default PIN | `const DEFAULT_PIN = '1234'` |
| Default notification message | Placeholder in `#tg-msg` input |

---

## License

MIT — do whatever you want with it.
