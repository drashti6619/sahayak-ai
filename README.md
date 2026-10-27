# 🎽 Sahayak AI — Live Monitoring Demo

> A hands-free safety assistant concept for visually impaired users: a wearable clip-on camera that continuously watches the surroundings and warns the user through **voice** and **vibration**.

This repository contains a **browser-based software simulation** of the wearable device. It uses your phone or laptop camera in place of the future hardware, so the full experience can be demonstrated with nothing but a web browser.

**🔗 Live demo:** `https://drashti6619.github.io/sahayak-ai/` *(replace after enabling GitHub Pages)*

---

## ✨ Features

- **Continuous monitoring**: the camera runs on its own; no pointing or holding needed.
- **Three alert levels**: 🟢 `SAFE`, 🟠 `CAUTION`, 🔴 `DANGER`, shown with a colour-coded banner.
- **Voice alerts**: warnings are read aloud using the browser's Speech Synthesis API.
- **Haptic feedback**: different vibration patterns for caution and danger (on devices that support it).
- **Alert log**: a timestamped history of every alert.
- **Demo buttons**: instantly trigger a simulated Danger or Caution alert, useful for presentations.
- **Zero dependencies**: a single `index.html` file. No build step, no server, no API keys.

---

## 🚀 Getting Started

### Option 1: Run locally

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

Browsers only allow camera access on **HTTPS** or **localhost**, so opening the file directly may not work. Start a tiny local server instead:

```bash
# Python 3
python -m http.server 8000
```

Then open **http://localhost:8000** and click **▶ Start Monitoring**.

### Option 2: GitHub Pages (recommended for demos)

1. Go to **Settings → Pages** in your repository.
2. Under **Source**, choose the `main` branch and the `/ (root)` folder.
3. Save. Your demo will be live at `https://<your-username>.github.io/<repo-name>/` over HTTPS, so the camera works on any phone.

> 📱 **On a phone:** open the GitHub Pages link, allow camera access, and the rear camera is used automatically.

---

## 🧠 How It Works

Every **2.5 seconds** the app grabs the current video frame, downsamples it to 64×48 pixels, and computes simple colour statistics:

| Signal | Calculation |
|---|---|
| Brightness | Average of the R, G and B channels |
| Red dominance | Average red minus the average of green and blue |

These values decide the alert level:

| Condition | Alert |
|---|---|
| Strong red dominance **and** bright frame (fire-like colours) | 🔴 **DANGER**: move away immediately |
| Very low brightness | 🟠 **CAUTION**: low light, proceed carefully |
| Otherwise | 🟢 **SAFE**, with an occasional 🟠 CAUTION message to show variety |

The result is displayed in the banner, written to the log, spoken aloud, and (for non-safe alerts) triggers a vibration.

---

## ⚠️ Limitations (Please Read)

This is a **proof-of-concept / hackathon MVP**, not a certified safety product.

- Detection is a **basic colour heuristic**, not a trained computer-vision model. A red object in good light can trigger a false "fire" alert, and real fires may be missed.
- Messages such as *"Person detected nearby"* are **randomly shown** to demonstrate the alert flow; the app does not actually detect people or measure distance.
- The *"Simulate Danger"* and *"Simulate Caution"* buttons use pre-written messages for demonstration only.
- **Do not rely on this for real-world navigation or safety.**

---

## 🗺️ Roadmap

- [x] **Phase 1**: Browser demo with live camera, voice and haptic alerts
- [ ] Replace the heuristic with a real object/hazard detection model (e.g. TensorFlow.js / YOLO) or a vision API
- [ ] Estimate distance and direction of obstacles
- [ ] Multi-language voice alerts (Gujarati, Hindi, English)
- [ ] **Phase 2: Hardware.** Run the same detection logic on an **ESP32-CAM** pendant clipped to a collar or lanyard, streaming to the phone over WiFi
- [ ] Offline mode and battery-optimised processing
- [ ] User testing with visually impaired users and accessibility organisations

---

## 🛠️ Tech Stack

- HTML5, CSS3, vanilla JavaScript
- `MediaDevices.getUserMedia`: live camera access
- Canvas 2D API: frame sampling and pixel analysis
- Web Speech API (`speechSynthesis`): spoken alerts
- Vibration API: haptic feedback

---

## 🌐 Browser Support

| Feature | Chrome / Edge | Safari (iOS) | Firefox |
|---|:---:|:---:|:---:|
| Camera | ✅ | ✅ | ✅ |
| Voice alerts | ✅ | ✅ | ✅ |
| Vibration | ✅ (Android) | ❌ | ✅ (Android) |

Vibration is not supported on iPhones, so the visual and voice alerts still work there.

---

## 🔒 Privacy

All processing happens **locally in your browser**. No video, image, or data is recorded, uploaded, or sent to any server.

---

## 🤝 Contributing

Ideas and pull requests are welcome. If you'd like to help with model integration, hardware design, or accessibility testing, please open an issue to start a conversation.

---

## 📄 License

Released under the [MIT License](LICENSE). *(Add a `LICENSE` file to your repo. GitHub can generate one for you via **Add file → Create new file → LICENSE**.)*

---

## 👤 Author

**Your Name**: [drashti6619](https://github.com/drashti6619) 

*Built with the goal of making independent mobility safer and more accessible for everyone.*
