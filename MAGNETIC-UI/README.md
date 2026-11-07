# MAGNETIC UI — The Magnetic Website

A no-backend, open-source web experiment that turns **real magnetic-field readings** into a soft, spring-driven interface response. On compatible hardware, bring a physical magnet near the device and the interface estimates the direction of the field change and moves toward it.

> **Important:** a magnetometer measures the local magnetic-field vector (X, Y, Z). It does not directly measure a magnet's screen position or distance. The four-edge calibration is an empirical approximation, not a precise position tracker. Sensor filtering, magnet polarity, device orientation, metal, speakers, cases and other magnetic sources can all affect the result.

## Features

- **REAL SENSOR:** uses `UncalibratedMagnetometer` when exposed, otherwise `Magnetometer`, from the Generic Sensor API. The app waits for an actual reading before declaring the sensor connected.
- **DEMO MODE:** pointer and touch simulate a force vector only inside the demo sandbox. Demo values never enter the sensor telemetry.
- **UNSUPPORTED:** clearly reports when the browser has no usable sensor or external connection API.
- Three-axis live telemetry, field magnitude, baseline delta, estimated direction and reaction intensity.
- First-run ambient baseline, plus a guided four-edge spatial calibration.
- Ridge-regularized 3D-to-2D mapping saved in browser `localStorage`.
- Spring/damping motion with force, sensitivity, smoothness and direction inversion controls.
- Optional external bridges: USB Serial JSON stream and a configurable Bluetooth LE GATT characteristic.
- Responsive layout, reduced-motion support, no required runtime dependencies and no backend.

## Run locally

The sensor APIs require a **secure context**. `localhost` is treated as secure by modern browsers; for a phone on your LAN, use an HTTPS deployment such as GitHub Pages.

```bash
git clone https://github.com/AtbeenLabs/AtbeenLabs.git
cd AtbeenLabs/MAGNETIC-UI
python3 -m http.server 8000
```

Open <http://localhost:8000>. The visual demo works without a sensor. For a real magnetometer, use a browser/device that exposes the API and grant permission when prompted.

No build step, package installation or server-side code is needed. `npm test` runs the calibration math tests with Node.js.

## Publish with GitHub Pages

This project is published inside the AtbeenLabs portfolio repository at `MAGNETIC-UI/`.

- **Source:** <https://github.com/AtbeenLabs/AtbeenLabs/tree/main/MAGNETIC-UI>
- **Live site:** <https://atbeenlabs.github.io/MAGNETIC-UI/>

The parent repository's GitHub Pages deployment must be enabled from its default branch and repository root. Once that deployment is active, the project is served from the live-site path above. GitHub Pages supplies HTTPS automatically.

All asset paths are relative, so the site also works when served from a repository subpath. GitHub Pages supplies HTTPS automatically.

## Real sensor behavior

1. Press **Activate real sensor** from a direct user click.
2. Allow magnetometer permission if the browser asks.
3. If there is no saved baseline, keep magnets away for the first 40 readings. The app averages them as an initial ambient baseline.
4. Bring a physical magnet near the device. The app calculates the vector difference from the baseline and scales the reaction by its magnitude.
5. Run the calibration wizard for a better screen-direction estimate.

The implementation requests a screen reference frame where available. If no four-point calibration is saved, X/Y are used as a **rough estimate** and the direction label is marked `EST.`. A calibrated mapping is preferred.

The app prefers `UncalibratedMagnetometer`, because its readings can preserve changes caused by nearby magnetic objects. If only the calibrated interface exists, the browser/OS may apply compensation that affects the response to external magnets. Hardware and browser behavior vary.

### Sensor security and compatibility

- The Generic Sensor APIs require a secure context (HTTPS or localhost), a visible/focused document, and user permission where required.
- The browser may block access through Permissions Policy. If your host sets a restrictive policy, allow `magnetometer` for the page origin.
- `Magnetometer` is experimental/limited availability and is not exposed by every major browser or device. Feature detection checks the API; only a received reading confirms a working sensor.
- The app does not use a compass, device orientation, pointer position or other heuristic to pretend a magnetometer exists.
- When the page is hidden, the app stops the sensor/bridge to release the resource and reduce power use. Reconnect after returning.

## External sensors on computers

A normal desktop/laptop browser cannot infer the presence of a magnetic sensor from the computer model. Many computers have no magnetometer. The external bridge supports explicit data protocols rather than guessing at arbitrary USB/Bluetooth devices.

### USB Serial

Connect a microcontroller or USB sensor bridge that emits **one UTF-8 JSON object per line** at 115200 baud:

```json
{"x": 1.2, "y": -0.4, "z": 38.1}
{"x": 1.3, "y": -0.5, "z": 38.0}
```

Values should be measured magnetic field components in microtesla (µT). The browser's Web Serial picker appears after pressing **Connect USB serial**. Web Serial is not available in every browser; it generally works in supported Chromium-based desktop browsers on HTTPS/localhost.

### Bluetooth LE (BLE)

The app accepts a custom GATT service UUID and characteristic UUID. The characteristic must send **UTF-8 JSON** in this same shape:

```json
{"x": 1.2, "y": -0.4, "z": 38.1}
```

The characteristic must support notifications. Browser Bluetooth support is limited, and not all Bluetooth sensor devices expose a compatible GATT service. This is not a universal Bluetooth scanner and cannot decode undocumented vendor-specific protocols automatically.

## Calibration guide

1. Keep the device fixed in its final screen orientation. Remove the magnet from the immediate area and capture the baseline.
2. Keep the same magnet pole facing the device and try to use a similar distance for each directional sample.
3. Capture the magnet beside the left, right, top and bottom edges in that order (or any order).
4. The app subtracts the baseline from each averaged 1.6-second sample, fits a ridge-regularized linear map from the three field axes to two screen axes, then stores the baseline and map in `localStorage`.
5. Recalibrate if the device rotates, the case/accessory changes, or the environment changes significantly.

A poor or noisy sample can make the estimate unstable. Keep the phone still and retry. One magnetometer cannot resolve every possible 3D magnet pose uniquely; this calibration only finds a useful approximation for a particular device, magnet and setup.

## Controls

- **Attraction strength:** scales maximum interface displacement.
- **Sensor sensitivity:** scales the reaction to the baseline delta; it does not alter the raw sensor values.
- **Motion smoothness:** adjusts spring stiffness/damping.
- **Invert direction:** reverses the estimated screen vector.
- **Reset:** restores tuning defaults and clears the saved calibration.
- **Demo mode:** uses pointer/touch position in the dedicated demo zone only. Sensor telemetry is cleared and the mode badge explicitly says `DEMO MODE`.

The `prefers-reduced-motion` media query disables the displacement animation for users who request reduced motion.

## Project structure

```text
magnetic-ui/
├── index.html
├── styles.css
├── favicon.svg
├── src/
│   ├── calibration.js   # baseline, regression, persistence
│   ├── main.js          # UI state, calibration flow and app wiring
│   ├── physics.js       # spring/damping motion engine
│   └── sensor.js        # Generic Sensor, Web Serial and Web Bluetooth
├── tests/
│   └── calibration.test.js
├── package.json
├── LICENSE
└── README.md
```

## Development checks

```bash
npm test
npm run check
```

## Privacy

No analytics, accounts, cookies, tracking or backend are used. Sensor readings stay in page memory. Only calibration data is stored locally in this browser via `localStorage`; the Reset button removes it.

## License

MIT. See [LICENSE](./LICENSE).

## Technical references

- [W3C Magnetometer specification](https://www.w3.org/TR/magnetometer/)
- [MDN Sensor APIs overview](https://developer.mozilla.org/en-US/docs/Web/API/Sensor_APIs)
- [MDN Magnetometer API](https://developer.mozilla.org/en-US/docs/Web/API/Magnetometer)
- [GitHub Pages documentation](https://docs.github.com/en/pages)
