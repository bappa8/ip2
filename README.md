# 🌐 IP Geolocation Lookup Tool

A sleek, dark-themed, single-page web app that looks up geolocation, network, and organization details for **any public IP address** (or your own) and pinpoints the approximate location on an interactive map.

Built with pure **HTML, CSS, and vanilla JavaScript** — no build step, no framework, no API key required.

---

## ✨ Features

| Category | Details |
|---|---|
| 🎨 **UI / Theme** | Deep-black gradient background with neon cyan & pink accents, glassmorphism cards, glowing buttons, and animated hover effects |
| 🔎 **IP Lookup** | Auto-detects your **public IP** on page load, or look up **any IPv4 / IPv6 address** |
| 🛰️ **Multi-Provider Fallback** | Tries **4 free IP-geo APIs** in sequence (`geojs.io` → `ipwho.is` → `freeipapi.com` → `ipapi.co`) so it keeps working even if one is blocked or rate-limited |
| 🗺️ **Interactive Map** | Real, smooth, draggable map powered by **Leaflet + OpenStreetMap** with a marker pin and popup |
| ➕ **Custom Zoom Controls** | Glowing **+ / −** zoom buttons that match the theme and auto-disable at min/max zoom |
| 📊 **Rich Info Table** | IP, Country (with flag emoji), City, Region, Latitude, Longitude, Time Zone, ASN, Organization |
| ⚡ **Performance** | Single HTML file, no build tools, ~13 KB of code, ~6-second per-provider timeout via `AbortController` |
| 📱 **Responsive** | Two-column desktop layout collapses to a single column on screens ≤ 850 px |
| 🚫 **Zero Config** | No API keys, no backend, no installation — just open `index.html` |

---

## 📸 Preview

```
┌──────────────────────────────────────────────────────────────┐
│  [ Enter an IP address...           ]   [  IP Lookup  ]     │
├────────────────────────────┬─────────────────────────────────┤
│  IP Address  8.8.8.8       │                                 │
│  Country     🇺🇸 United…   │           🗺️  Map               │
│  City        Mountain View │            📍 Pin               │
│  Region      California    │                              + │
│  Latitude    37.42301      │                              − │
│  Longitude   -122.083352   │                                 │
│  Time Zone   America/LA    │                                 │
│  ASN         AS15169       │                                 │
│  Organization Google LLC   │                                 │
└────────────────────────────┴─────────────────────────────────┘
                  © Bappa Ghosh. All rights reserved.
```

---

## 🚀 Getting Started

### Option 1 — Just open the file
1. Download / clone the project.
2. Double-click **`index.html`** to open it in any modern browser (Chrome, Firefox, Edge, Safari, Brave).
3. Your public IP info will load automatically. 🎉

### Option 2 — Serve locally (recommended for development)
A local web server avoids any quirks with `file://` URLs.

```bash
# Using Python (no install needed on most systems)
cd ip-lookup
python3 -m http.server 8000

# Then visit:
# http://localhost:8000
```

Or with Node:
```bash
npx serve .
```

---

## 🛠️ Project Structure

```
ip-lookup/
├── index.html      # The entire app — HTML + CSS + JS in one file
└── README.md       # You are here
```

That's it. Truly self-contained.

---

## 🧩 How It Works

### 1. Fetching IP Data
On page load (or when you click **IP Lookup**), the app calls a chain of free IP-geolocation APIs in order. The first one that returns valid data wins — the rest are skipped.

```js
const providers = [
  { name: 'geojs.io',       url: ... },   // primary
  { name: 'ipwho.is',       url: ... },   // fallback 1
  { name: 'freeipapi.com',  url: ... },   // fallback 2
  { name: 'ipapi.co',       url: ... },   // fallback 3
];
```

Each provider has a `map()` function that **normalizes** its response into a common shape:
```js
{ ip, city, region, country_code, country_name,
  latitude, longitude, timezone, tz_offset, asn, org }
```

A 6-second `AbortController` timeout prevents any single slow provider from freezing the chain.

### 2. Rendering the Map
[Leaflet.js](https://leafletjs.com) renders an interactive map with **OpenStreetMap** tiles. When IP data arrives, the map smoothly flies to the coordinates and drops a marker pin with a popup.

### 3. Custom Zoom Controls
The default Leaflet zoom UI is hidden via CSS, and two custom glowing buttons call:
```js
map.zoomIn();
map.zoomOut();
```
Buttons are auto-disabled when you reach the map's min/max zoom level.

---

## 🌐 APIs & Libraries Used

| Service / Library | Purpose | Auth | License |
|---|---|---|---|
| [geojs.io](https://www.geojs.io/) | Primary IP geolocation | None | Free |
| [ipwho.is](https://ipwho.is/) | Fallback IP geolocation | None | Free |
| [freeipapi.com](https://freeipapi.com/) | Fallback IP geolocation | None | Free |
| [ipapi.co](https://ipapi.co/) | Fallback IP geolocation | None | Free (rate-limited) |
| [Leaflet 1.9.4](https://leafletjs.com/) | Interactive map library | None | BSD-2-Clause |
| [OpenStreetMap](https://www.openstreetmap.org/) | Map tile data | None | ODbL |

---

## 🎨 Customization

Open `index.html` and tweak these CSS variables / blocks to make it yours:

| What | Where |
|---|---|
| Background gradient | `body { background: linear-gradient(...) }` |
| Accent colors | Search for `#00f2fe` (cyan) and `#f093fb` (pink) |
| Label / value font sizes | `table td:first-child` / `table td:last-child` |
| IP "headline" style | `table td.ip-value` |
| Zoom-button look | `.zoom-btn` |
| Footer text | `<footer>© Bappa Ghosh. All rights reserved.</footer>` |

---

## 🐛 Troubleshooting

| Issue | Likely cause / fix |
|---|---|
| ⚠️ "Failed to fetch IP data" | A browser **ad-blocker** (uBlock, Brave Shields, Firefox strict mode) is blocking the IP-geo APIs. Disable the blocker for the page or whitelist `geojs.io`, `ipwho.is`, `freeipapi.com`, `ipapi.co`. |
| Map shows but tiles are blank | Your network is blocking `tile.openstreetmap.org`. Try a different network or VPN. |
| Map is grey / not interactive | Open the file via a local server (see *Getting Started*). Some browsers restrict CDN scripts under `file://`. |
| Wrong city / location | IP geolocation is **approximate** — accuracy depends on the provider's database. VPNs, mobile carriers, and corporate networks often show inaccurate cities. |

---

## 🔒 Privacy

- The page runs **entirely in your browser** — no analytics, no tracking, no backend of ours.
- IP lookup requests are sent **directly from your browser** to the third-party APIs listed above. Their respective privacy policies apply.
- No data is stored, logged, or transmitted anywhere else.

---

## 📦 Browser Support

Works in all evergreen browsers released in the past 4 years:
- ✅ Chrome / Edge 90+
- ✅ Firefox 90+
- ✅ Safari 14+
- ✅ Brave / Opera / Vivaldi (latest)
- ✅ Mobile Chrome / Safari

Requires JavaScript enabled.

---

## 🗺️ Roadmap Ideas

- [ ] Toggle between **dark / light theme**
- [ ] Copy-to-clipboard button on each row
- [ ] **History** of recent lookups (localStorage)
- [ ] **Bulk lookup** (paste multiple IPs)
- [ ] Export results as **JSON / CSV**
- [ ] Add **dark-mode map tiles** (CartoDB Dark Matter) to match the theme
- [ ] **Reverse DNS** / hostname lookup
- [ ] Show **ISP, proxy/VPN, and threat-intel flags**

PRs welcome!

---

## 📄 License

This project is released under the **MIT License** — feel free to use, modify, and distribute.

---

## 👤 Author

**Bappa Ghosh**

> © Bappa Ghosh. All rights reserved.

If you find this useful, give the repo a ⭐ — and feel free to fork it and make it your own!
