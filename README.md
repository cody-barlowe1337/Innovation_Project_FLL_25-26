BOB - Field Cataloger & Analyzer

Purpose
-------
BOB is a lightweight browser-based application designed to assist archaeologists in the field by automatically cataloging photographed objects using AI. The app captures images from a camera, sends images to a generative AI (Gemini / Generative Language API) for classification and description, and stores artifact records in Firebase Firestore. It also provides export options (PDF, XLSX, JSON) and a small in-browser catalog UI.

Key Features
------------
- Capture images from device camera (getUserMedia) and preview before submitting.
- AI analysis of images to classify whether the object is an archaeological artifact and provide a short description and material.
- Persistent storage using Firebase Firestore (real-time listeners, authentication).
- Export individual artifacts as PDF/XLSX and bulk export (XLSX/JSON).
- Local development mode: the app can run with no Firebase or Gemini keys for quick testing; a mock analyzer will produce deterministic responses.

Technical Stack
---------------
- Single-file front-end: `index.html` (HTML, CSS, JS)
- Tailwind CSS via CDN for styling
- Firebase Web SDK (v11.6.1) for Firestore and Authentication
- Generative Language API (Gemini) via REST for image analysis
- jsPDF for PDF export
- SheetJS (xlsx) for Excel exports

Important Files
---------------
- `index.html` - Main application file. Contains the entire UI and client-side logic. Key sections:
  - Head: loads Tailwind, fonts, jsPDF, SheetJS, and Firebase SDKs.
  - Inline script: handles Firebase initialization, camera capture, AI analysis (calls Gemini API or uses a local mock), Firestore reads/writes, catalog UI, and export functions.

Running Locally (recommended for testing)
----------------------------------------
1. Clone the repo or open the project folder locally.
2. Serve the folder over a local server (recommended for camera access):

   Using Python 3:

   ```bash
   python3 -m http.server 8000
   ```

   Or using node http-server (install if needed):

   ```bash
   npm install -g http-server
   http-server -c-1
   ```

3. Open `http://localhost:8000` in a modern browser and allow camera access when prompted.

Local dev defaults
------------------
- `index.html` contains a small inline script that sets `window.__firebase_config = '{}'` and `window.__gemini_api_key` to a test key when those variables are not provided by the environment. This enables a local "mock" analysis path so you can exercise camera capture and UI without a backend.
- In local mode (no Gemini API key), the app uses a mock analyzer that returns simulated results so you can test saving/exporting flows.

Firebase setup (optional for full features)
------------------------------------------
To enable real persistence and real-time catalog sync, create a Firebase project and add a web app (Firebase Console → Project settings → Your apps → Add app):

1. Copy the Firebase config object and inject it into the page in one of two ways:
   - For local testing, replace the empty default in the inline script in `index.html` with your config object as a JSON string assigned to `window.__firebase_config`.
   - For production, inject the config via your hosting environment (do not commit it to a public repo).

2. Ensure Firestore is enabled in the project and configure security rules.

3. The app uses anonymous or custom auth tokens. If you use anonymous auth, the client will sign in automatically; otherwise provide a token via `__initial_auth_token`.

Gemini (Generative Language) API
--------------------------------
- The app expects a Gemini API key to call the Generative Language REST endpoint. For local testing, the repository includes a test key in `index.html` when no environment value is provided.
- IMPORTANT: Do not store unrestricted production keys in client code. For production, place keys on a secure server or use a serverless proxy (Cloud Functions, Vercel, Netlify functions) that calls the API and returns results to the client.

Security notes
--------------
- Firebase config is not secret, but API keys for the Generative Language API are sensitive and should be protected.
- If deploying the app publicly (e.g., GitHub Pages), remove the key from `index.html` and use a server-side proxy or other secure method.

Deploy to GitHub Pages
---------------------
1. Commit your changes to the `main` branch and push to GitHub.

```bash
git add index.html README.md
git commit -m "Add local defaults and README"
git push origin main
```

2. In GitHub, go to the repository Settings → Pages and select the `main` branch and `/ (root)` folder. Save and wait a minute for the site to publish.

3. Visit the provided `github.io` URL.

Notes & Next Steps
------------------
- If you'd like, I can:
  - Add a simple localStorage fallback so the catalog persists across reloads without Firebase.
  - Remove the embedded API key and scaffold a serverless proxy (example for Vercel/Netlify/Google Cloud Functions) to keep the key secret.
  - Add unit tests or a minimal smoke test harness.

Contributors
----------------------
Vihaan Parmar,
Deetyam Soni,
Akshaj Narayan,
and Yuvansh Haval


Contact / Contribution
----------------------
Open an issue or pull request with improvements. Include device/browser details when reporting camera-related issues.
