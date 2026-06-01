# HyperX Battery — Stream Deck Plugin

> Real-time battery level for your **HyperX Cloud 3S Wireless** headset, displayed directly on a Stream Deck key.

![Version](https://img.shields.io/badge/version-1.0.0-blue?style=flat-square)
![Platform](https://img.shields.io/badge/platform-Windows-lightgrey?style=flat-square)
![Stream Deck](https://img.shields.io/badge/Stream%20Deck-6.8%2B-black?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)

---

## Features

- **Live battery percentage** — polls your headset every 60 seconds via direct HID communication
- **Color-coded indicator** — green above 50%, yellow at 20–50%, red below 20%
- **Proportional battery bar** — fills visually as charge changes
- **Disconnect detection** — shows `--` cleanly when the headset is off or out of range
- **No middleware** — talks to the headset directly over HID; no iCUE or G HUB required
- **Zero config** — drop the action onto your deck and it works immediately

---

## Compatibility

| Headset | Supported |
|---|---|
| HyperX Cloud 3S Wireless | ✅ |
| All other headsets | ❌ |

**OS:** Windows 10 / 11  
**Stream Deck software:** 6.8 or newer

---

## Installation

### Stream Deck Plugin
<!-- replace coming soon with recommended after its uploaded -->
**From the Elgato Marketplace** *(coming soon)* — This plugin is not yet published on the Elgato Marketplace. For now, please use the manual installation steps below. 
<!-- search for **"HyperX Battery"** in the Stream Deck software's Plugin Store, or find it on the [Elgato Marketplace](https://marketplace.elgato.com). -->

**Manual:**
1. Download the latest `.streamDeckPlugin` from [Releases](https://github.com/JAG-Twinster/StreamDeck-HyperX-Cloud-3S-Battery-Level/releases/tag/release)
2. Double-click the file — Stream Deck software installs it automatically
3. Drag the **HyperX Battery** action onto any key

If the above steps do not work, manually copy the **hyperx-cloud-3s-battery.sdPlugin** into your streamdeck plugins folder

---

## How It Works

The plugin opens the HyperX Cloud 3S Wireless USB dongle as a raw HID device (VID `0x03F0`, PID `0x06BE`, usage page `0xffc0`) and sends a 52-byte status request every 60 seconds. Byte 4 of the response contains the battery level (0–100). The key image is rendered as an SVG and pushed to Stream Deck over the plugin WebSocket API.

A 3-strike failure buffer prevents momentary read errors (e.g. from G HUB briefly claiming the device) from flashing a disconnect state.

---

## Development

```
hyperx-battery-streamdeck/
└── com.twinster.hyperx-cloud-3s-battery.sdPlugin/
    ├── manifest.json   # Plugin metadata & action definitions
    ├── app.js          # Main plugin logic (Node.js)
    ├── launcher.bat    # Entry point called by Stream Deck
    ├── package.json
    └── images/         # Action & category icons
```

**Dependencies:** [`ws`](https://github.com/websockets/ws), [`node-hid`](https://github.com/node-hid/node-hid)

```bash
cd com.twinster.hyperx-cloud-3s-battery.sdPlugin
npm i
```

Stream Deck launches `launcher.bat` with connection arguments (`-port`, `-pluginUUID`, `-registerEvent`). The plugin registers itself over WebSocket and begins polling on `willAppear`.

---

## Credit

This project started from the excellent **hyperx-battery-streamdeck** plugin by **Mihajlo Kuzmanoski**. Huge thanks and full credit to Mihajlo for the original work, architecture, and inspiration.

Original repository: https://github.com/mihajlo-kuzmanoski/hyperx-battery-streamdeck

## License

MIT — see [LICENSE](LICENSE)
