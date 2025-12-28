# SmartResumeFix-49

Rule-based Node.js + Express app that converts plain resume text into a clean, professional PDF.
No paid AI and no headless browsers (no Puppeteer) — purely rule-based PDF generation with pdfkit. Designed to be small and deployable on Replit, Render, Railway, or any Node.js host.

## Features
- Paste plain resume text in the browser and generate a PDF.
- Small, dependency-light server using Express + PDFKit.
- Simple frontend for quick testing.
- Routes:
  - `POST /api/generate` — generate a PDF from resume text.
  - `POST /api/test-generate` — test route for health/static checks.
  - `GET /health` — health check.

## Quick start

Prerequisites:
- Node.js 18+ (project sets engines to >=18)

Install and run locally:

```bash
git clone https://github.com/armankunhi-oss/SmartResumefix-49.git
cd SmartResumefix-49
npm install
cp .env.example .env        # edit values if you plan to use optional features
npm start
```

Open your browser at http://localhost:5000 and use the UI to paste resume text and generate a PDF.

## API

- Generate PDF
  - Endpoint: `POST /api/generate`
  - Content-Type: `application/json`
  - Body:
    - `resumeText` (string, required) — plain resume text to convert
    - `targetRole` (string, optional) — title to display at top of PDF
    - `resumeType` (string, optional) — style choice (e.g. "Modern Professional")
  - Response: `{ success: true, pdfPath: "/resumes/<uuid>.pdf" }` on success

Example using curl:

```bash
curl -X POST http://localhost:5000/api/generate \
  -H "Content-Type: application/json" \
  -d '{"resumeText":"Your resume text here","targetRole":"Software Engineer","resumeType":"Modern Professional"}'
```

- Test route
  - Endpoint: `POST /api/test-generate`
  - Returns a small JSON message confirming rule-based generation is working.

- Health
  - Endpoint: `GET /health`
  - Returns `{ "ok": true }`.

## Environment variables

Fill a `.env` (see `.env.example`) for optional features:

- RAZORPAY_KEY_ID, RAZORPAY_KEY_SECRET — placeholders (not required for basic PDF generation)
- HOST_URL — used for links if needed (default: `http://localhost:5000`)
- EMAIL_SMTP_HOST, EMAIL_SMTP_PORT, EMAIL_SMTP_USER, EMAIL_SMTP_PASS — optional email settings
- DELIVERY_EMAIL_FROM — optional sender address

Note: The app runs fine without any of these for the core PDF-generation functionality.

## Project structure (important files)

- `server.js` — main Express server and PDF generation logic (pdfkit)
- `public/` — static frontend
  - `index.html`, `css/style.css`, `js/app.js`
- `resumes/` — generated PDFs (ignored in .gitignore)
- `.env.example` — example environment variables

## Deployment

This app is lightweight and works on typical Node.js hosts:

- Render / Railway / Replit: configure to run `npm start` and ensure Node 18+.
- Make sure the `resumes/` directory is writable by the process (it is created automatically on startup).
- For production, consider:
  - Storing generated PDFs in object storage (S3, etc.) if persistence across deploys is needed.
  - Adding rate-limiting and input validation if open to the public.

## Contributing

PRs and issues are welcome. Keep changes small and focused — this project is intentionally simple.

## License

MIT

## Notes / Next steps
- The current PDF generation is intentionally minimal (simple font sizes and text flow). If you'd like:
  - Improved parsing of resume sections (Experience, Education, Skills).
  - Multiple template styles with better layout.
  - Option to email the generated PDF.
  - Save to cloud storage instead of local `resumes/`.
I can add any of these in follow-up changes if you'd like — tell me which and I will implement it.
