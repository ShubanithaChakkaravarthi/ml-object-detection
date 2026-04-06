🔍 ML Object Detection + ESP32
A fully browser-based real-time object detection app powered by TensorFlow.js (COCO-SSD) — no API key, no server, no install. Works on desktop and mobile. Optionally triggers an ESP32 LED when a target object is detected.
![TensorFlow.js](https://img.shields.io/badge/TensorFlow.js-4.17.0-orange?logo=tensorflow)
![COCO-SSD](https://img.shields.io/badge/Model-COCO--SSD-blue)
![No API Key](https://img.shields.io/badge/API%20Key-None%20Required-brightgreen)
![Mobile Ready](https://img.shields.io/badge/Mobile-Ready-success)
---
✨ Features
80 object classes detected using COCO-SSD (person, car, cat, dog, bottle, laptop, and more)
Webcam live detection — auto-scan every 2 seconds
Image upload & analyze — drag & drop or browse from gallery / camera
Object filter — detect only what you care about, hide the rest
Quick presets — Traffic, Hazards, Person, Animals, Items
ESP32 LED control — auto-trigger LED on detection via HTTP
Notifications — on-screen alert, sound beep, email (simulated)
Dark / Light theme toggle
Mobile friendly — back camera default, front/back flip button, touch support
100% client-side — runs entirely in your browser, nothing is sent to any server
---
🚀 Quick Start
Option 1 — Open directly (Image Upload only)
Just download `ml_object_detection_v7_mobile.html` and open it in any modern browser. Image upload and detection will work immediately.
Option 2 — Host on GitHub Pages (Full features including Camera)
Fork or upload this repo to GitHub
Go to Settings → Pages
Set source to `main` branch, root folder
Visit `https://yourusername.github.io/your-repo-name/ml_object_detection_v7_mobile.html`
> **Note:** Live webcam requires **HTTPS**. GitHub Pages provides HTTPS automatically, making it the easiest way to use all features on mobile.
Option 3 — Local server
```bash
# Python
python -m http.server 8080

# Node.js
npx serve .
```
Then open `http://localhost:8080/ml_object_detection_v7_mobile.html`
---
📱 Mobile Usage
Feature	Works on Mobile
Image Upload / Analyze	✅ Always
Take Photo with Camera	✅ Via upload button
Live Webcam Detection	✅ On HTTPS only
Front / Back Camera Flip	✅ Flip button
ESP32 Control	✅ On same WiFi network
> If you see a camera error on mobile, make sure you're on an **HTTPS** URL — browsers block camera access on plain `http://`.
---
🎯 Detected Object Classes (80 total)
Person, bicycle, car, motorcycle, airplane, bus, train, truck, boat, traffic light, fire hydrant, stop sign, parking meter, bench, bird, cat, dog, horse, sheep, cow, elephant, bear, zebra, giraffe, backpack, umbrella, handbag, tie, suitcase, frisbee, skis, snowboard, sports ball, kite, baseball bat, baseball glove, skateboard, surfboard, tennis racket, bottle, wine glass, cup, fork, knife, spoon, bowl, banana, apple, sandwich, orange, broccoli, carrot, hot dog, pizza, donut, cake, chair, couch, potted plant, bed, dining table, toilet, TV, laptop, mouse, remote, keyboard, cell phone, microwave, oven, toaster, sink, refrigerator, book, clock, vase, scissors, teddy bear, hair drier, toothbrush
---
⚡ ESP32 Integration
The app can trigger an ESP32 LED over your local WiFi network when a target object is detected.
ESP32 Arduino Sketch (basic)
```cpp
#include <WiFi.h>
#include <WebServer.h>

const char* ssid     = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
const int   LED_PIN  = 2;

WebServer server(80);

void handleLed() {
  String state = server.arg("state");
  if (state == "on")    digitalWrite(LED_PIN, HIGH);
  else if (state == "off")  digitalWrite(LED_PIN, LOW);
  else if (state == "blink") {
    for (int i = 0; i < 6; i++) {
      digitalWrite(LED_PIN, !digitalRead(LED_PIN));
      delay(200);
    }
  }
  server.send(200, "text/plain", "OK");
}

void setup() {
  pinMode(LED_PIN, OUTPUT);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) delay(500);
  server.on("/led", handleLed);
  server.begin();
}

void loop() { server.handleClient(); }
```
Enter your ESP32's IP address in the app under Hardware Control → ESP32 IP Address, then click Test to verify.
---
🛠️ Tech Stack
Technology	Purpose
TensorFlow.js 4.17	ML inference in browser
COCO-SSD 2.2.3	Object detection model
Space Mono + Syne	Fonts
Vanilla HTML/CSS/JS	No framework needed
---
📁 File Structure
```
/
├── ml_object_detection_v7_mobile.html   # Main app (single file)
└── README.md                            # This file
```
Everything is in a single HTML file — no build step, no dependencies to install.
---
🔒 Privacy
All processing happens 100% in your browser. No images, video frames, or detection results are ever sent to any server. The only network requests are:
Loading TensorFlow.js and COCO-SSD model from CDN (on first load, ~5MB)
ESP32 LED requests to your local network IP (if configured)
---
📄 License
MIT — free to use, modify, and distribute.
