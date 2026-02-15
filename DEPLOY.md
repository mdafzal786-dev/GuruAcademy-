# Deployment notes

Summary:
- Frontend: deploy to Netlify (connect `frontend/` folder). Build command: `npm run build`. Publish directory: `dist`.
- Backend: deploy to Render (select `server/` directory). Start command: `npm start` (Render sets `PORT`).

Environment variables to add on the hosting platforms (values from `.env` / secret manager):

- Server (`server/.env.example`):
  - `PORT` (Render provides this automatically)
  - `DB` or `MONGODB_URI` (MongoDB connection string)
  - `JWT_SECRET`, `activation_Secret`
  - `Gmail`, `Password` (email account or app password for nodemailer)
  - `Razorpay_key`, `Razorpay_Secret`
  - `GEMINI_API_KEY` (if using chatbot)
  - `frontendurl` (set to your Netlify URL)

- Frontend (Netlify site settings):
  - `VITE_SERVER_URL` (point to your Render service, e.g. https://your-backend.onrender.com)
  - `VITE_RAZORPAY_KEY` (Razorpay public key used by client)

Quick checklist:
- [ ] Add all secrets to Render and Netlify dashboards — never commit secrets to Git.
- [ ] Ensure `frontend/public/_redirects` exists so client routes work on Netlify.
- [ ] Update frontend `server` constant to use `import.meta.env.VITE_SERVER_URL` instead of hardcoded `http://localhost:5000` before or after deployment.
- [ ] Replace hardcoded Razorpay test key in the frontend with `VITE_RAZORPAY_KEY`.

Optional improvements:
- Add `engines.node` to `server/package.json` to pin Node version for Render.
- Normalize database env variable name to `MONGODB_URI` in `server/database/db.js` (currently it reads `DB`).
