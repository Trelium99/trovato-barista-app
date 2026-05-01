# ☕ Barista Bell — Customer Call Kiosk

A lightweight, static single-page kiosk app for cafés. Customers tap a button to notify baristas via ntfy push notifications. No backend, no subscriptions, no accounts — just a static HTML file.

---

## How It Works

1. The iPad sits on the counter displaying the kiosk
2. A customer taps **Call Barista**
3. A push notification is sent instantly to every barista's phone via ntfy
4. The barista heads over — no need to be at the counter 24/7

---

## Stack

- Pure HTML / CSS / JS — zero dependencies, zero build step
- [ntfy.sh](https://ntfy.sh) for free push notifications
- Hosted as a **free static site** on [GitHub Pages](https://pages.github.com)
- Credentials stored in `localStorage` — local to the iPad only, never sent to any server other than ntfy.sh

---

## Deployment — GitHub Pages

### 1. Push to GitHub

```bash
git init
git add index.html README.md
git commit -m "init"
git remote add origin https://github.com/YOUR_USERNAME/barista-kiosk.git
git push -u origin main
```

### 2. Enable GitHub Pages

1. Go to your repo on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under **Source** select **Deploy from a branch**
4. Set branch to **main** and folder to **/ (root)**
5. Click **Save**

Your app will be live within 60 seconds at:
```
https://YOUR_USERNAME.github.io/barista-kiosk/
```

**Cost: Free.**

---

## ntfy Setup

### Step 1 — Install the ntfy app

Each barista installs the free **ntfy** app on their phone:
- iOS: [App Store](https://apps.apple.com/app/ntfy/id1625396347)
- Android: [Google Play](https://play.google.com/store/apps/details?id=io.heckel.ntfy) or [F-Droid](https://f-droid.org/en/packages/io.heckel.ntfy/)

### Step 2 — Choose a topic name

Pick a unique topic name that's hard to guess — anyone who knows the topic can receive (and send) notifications:
```
cafebell-x7f2
```
There's no signup required. The topic is created automatically the first time something posts to it.

### Step 3 — Subscribe in the app

Each barista opens the ntfy app and taps **Subscribe to topic** and enters the topic name exactly.

### Step 4 — Configure the kiosk on the iPad

1. Open the GitHub Pages URL in Safari on the iPad
2. Tap **⚙ Setup** (bottom right)
3. Enter the PIN (default: **1234**)
4. Enter the **topic name** and optionally customise the notification message
5. Tap **Save & Activate**

Tap the bell button to test — all subscribed phones should receive a notification instantly.

---

## PIN Protection

The setup screen is protected by a 4-digit PIN to prevent customers accessing configuration.

- **Default PIN:** `1234`
- **To change it:** Tap Setup → enter current PIN → scroll to **Change PIN** → enter new PIN → tap **Update PIN**

---

## iPad Kiosk Mode

Lock the iPad to only show the kiosk (recommended for production):

1. Go to **Settings → Accessibility → Guided Access**
2. Enable **Guided Access** and set a Guided Access passcode
3. Open Safari and navigate to your GitHub Pages URL
4. Triple-press the **Side / Home button** to start Guided Access
5. Tap **Start**

The iPad is now locked to that one page. Triple-press the side button and enter the passcode to exit.

---

## Notification Cooldown

After a customer taps the bell the button is disabled for **8 seconds** to prevent notification spam. Adjust by changing the `8000` value (milliseconds) in the `callBarista` function in `index.html`.

---

## File Structure

```
barista-kiosk/
├── index.html    ← the entire app
└── README.md     ← this file
```

---

## Customisation

| What | Where in `index.html` |
|---|---|
| Welcome heading | `<h1>Need a <em>Barista?</em></h1>` |
| Hint text | `<p class="hint">...</p>` |
| Notification cooldown | `8000` in `setTimeout` inside `callBarista()` |
| Default PIN | `const DEFAULT_PIN = '1234'` |

---

## Security Notes

- The ntfy topic name is stored in `localStorage` which is **local to the device and browser** — visiting the same URL on a different device shows an unconfigured kiosk
- Keep the topic name reasonably random — anyone who knows it can post to it or subscribe to it
- For higher security ntfy supports [access control](https://docs.ntfy.sh/config/#access-control) if you self-host

---

## License

MIT — do whatever you want with it.
