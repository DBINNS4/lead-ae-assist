
# 🎬 Lead AE Assist Beta v1.0

Lead AE Assist Beta v1.0 is a professional-grade, modular desktop app built to automate ingesting, transcoding, and managing media workflows. Your assistant for smart ingest, prep, and project organization — made for AEs.

---

## 🚀 Features

- ✅ Modular panels: Ingest, Transcode, and more
- ✅ Silent license-based feature gating
- ✅ Cross-platform (macOS, Windows, Linux)
- ✅ FFmpeg-powered transcoding with AI-enhanced suggestions
- ✅ Multi-TB ingest ready with hashing, deduplication, and retry
- ✅ Real-time preview with crop, LUT, and in/out markers
- ✅ New Transcribe panel for speech-to-text
- ✅ JSON/Avid transcriptions use OpenAI's verbose_json for detailed segments
- 📄 [FFmpeg Build Plan](docs/FFmpeg_Build_Plan.md) lists the bundled codecs and containers
- 📄 [Transcription and Reconciliation](docs/transcription.md) covers dual-engine flags, workflow, and outputs

---

## 🧩 Modular Architecture

Lead AE Assist Beta v1.0/
├── index.html
├── style.css
├── main.js
├── renderer.js
├── renderer.ingest.js
├── renderer.transcode.js
├── modules/
│   ├── ingest.js
│   └── transcode.js
├── utils/
│   ├── hash.js
│   ├── validate.js
│   └── path.js
├── config/
│   └── state.json
│     (single source of truth for preferences and licensing)
├── README.md

---

## 🛠 Setup

npm install
npm start

`npm start` expects the environment variable `NODE_ENV` to be set to
`development`. The startup script also looks for `DEBUG_UI`; when set to `true`
the Electron DevTools will open alongside the main window. Windows users may
need the `cross-env` utility to set these variables.

macOS 14 and later require the JIT entitlement `com.apple.security.cs.allow-jit`
in your signing configuration. Without it, Electron will fail to launch.

---

## ⚙️ Panels

### 🔹 Ingest
- Dual-destination copy with checksum and retry
- Deduplication using fast hashing (xxHash, Blake3)
- Optional logs, structure templates, silent fail recovery
- Can rename source files to `.done` after ingest when watching folders
- Stopping watch mode will restore those `.done` files back to their original names
- Selecting a single folder sets it as the ingest source and clears any file selections

### 🔹 Transcode
- ProRes, H.264/5, AV1, VP9, FFV1, etc.
- MXF and MOV output for Avid and Premiere
- Image sequences (PNG, TIFF, EXR, TGA)
- Safe zone overlay, LUT preview, crop + trim
- CRF, CBR, 2-pass, GPU encoding
- AI format suggestion and retry logic
- Unified summary with per-file progress bar

### 🔹 Transcribe
- Enable `dualEngine` to run both Lead AE Assist Beta v1.0 and WhisperX for cross-checking results.
- Use `preferEngine` to choose which engine's words are kept when reconciling.
- `autoConfidence` automatically accepts the higher-confidence word during reconciliation.
- Outputs include TXT, SRT, VTT, JSON, XML, script, and burn-in subtitle files.
- A reconciliation overlay highlights discrepancies so you can review or apply preferences quickly.

### Quick Tips
- Use the **Reset** button on any panel to clear all fields.
  Drop-down menus return to a simple "-- Select --" prompt so you can start over.


## 🧠 AI Integration

Commands like:
- “Transcode for Instagram”
- “Ingest this to MXF and prep for Avid”
- “Retry failed files with CRF 18”

---

## 🔒 Licensing

Edit `state.json` in your application data folder to change access and manage preferences:

"licenseTier": "pro"

Tiers: free, pro, enterprise  
Features silently lock if unavailable.

---

## 📦 Coming Soon

- Cloud delivery + analytics hooks

---

## 🤝 Contributing

Before running `npm test`, run `npm install` so `node_modules` is populated and
the `jest` CLI is available. If you're working offline, install dependencies
ahead of time or ensure an offline npm cache is present.

---

## 🧪 Testing

Run `npm install` before `npm test` so all dependencies are available. Tests
require either internet access or an offline npm cache to complete
installation.

## 💬 Support

This version is hand-built for professional post workflows.
No telemetry. No cloud sync. No BS.

Use it. Break it. Rebuild it.
**Lead AE Assist Beta v1.0 is your new assistant editor.**

---

## 🛠️ Troubleshooting

- **Root folder checkbox not recognized in Clone panel**

  Older builds used the absolute path for folder tree data attributes,
  causing the root checkbox query (`data-path=""`) to fail. Versions
  after this fix use the relative path so the root selection works
  correctly.

- **EPERM errors when scanning drives on macOS**

  Scanning a drive that contains protected system folders (e.g.,
  `.DocumentRevisions-V100`) may raise an `EPERM` error. The folder tree
  utility now skips unreadable folders so the scan can continue.

- **Electron fails to launch on macOS 14+**

  macOS builds must include the JIT entitlement
  `com.apple.security.cs.allow-jit`; without it, Electron will not start.

- **Bundled ffprobe not found**

  The app expects an ffprobe binary at `extra/ffmpeg/ffprobe`. Verify the
  file exists and is executable (`chmod +x` on macOS/Linux). Errors like
  `spawn ... ffprobe ENOENT` mean the path or permissions are incorrect.
  Windows users may need the `ffprobe.exe` version in the same directory.

---

## Third-Party Notices

Lead AE Assist Beta v1.0 uses FFmpeg, a free and open-source software licensed under the LGPLv2.1.
FFmpeg project: <https://ffmpeg.org>
Binaries from: <https://github.com/BtbN/FFmpeg-Builds>
