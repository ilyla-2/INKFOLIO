# INKFOLIO — private authoring studio

This repository is being prepared to host the uploaded InkFolio Cloud app from Google AI Studio.

## Privacy prerequisite

**This repository is currently PUBLIC.** Do not upload `inkfolio-chantless-magi-private-deploy.zip`, `src/data/chantlessMagiChaptersData.json`, or other private manuscript materials until the repository has been changed to **Private** in GitHub:

**Repository → Settings → General → Danger Zone → Change repository visibility → Make private**

A password on the deployed website does **not** prevent anyone from reading files committed to a public GitHub repository.

## Install the application source

After switching to private, upload the **extracted contents** of the supplied ZIP into the repository root (not the ZIP as one file). The application uses Node 22+, React 19, Vite 6, TypeScript, Express, and a package lock.

```bash
npm ci
npm run verify
```

The repository should then contain `package.json`, `package-lock.json`, `src/`, `public/`, `server.ts`, `vite.config.ts`, and `Dockerfile` at root. Do not commit `.env` or real credentials.

## Google AI Studio Build

1. In Google AI Studio **Build**, choose **Add files (+) → Import from GitHub** and select this repository and `main`.
2. Inspect the imported app. The app's full-stack runtime must serve `server.ts` (or an equivalent protected server) — not expose a bare `dist` folder.
3. Configure `PRIVATE_ACCESS_USER` and a strong `PRIVATE_ACCESS_PASS` **only** in server-side secrets/runtime environment.
4. Ensure the production server uses `REQUIRE_PRIVATE_AUTH=1` or equivalent fail-closed authentication. The ZIP's Dockerfile sets this environment variable; verify whether AI Studio's publish process honors the Dockerfile.
5. Publish to Cloud Run. Verify that unauthenticated requests to `/`, `/assets/*`, and `/art/*` return 401/403 (or 503 when intentionally locked), and authenticated access works.
6. For stronger private-only hosting, set Cloud Run ingress/IAM authentication as appropriate, rather than relying only on HTTP Basic authentication.

**The repository is not a deployment by itself.** The Google AI Studio GitHub connection and Cloud Run publication are separate steps.

See `DEPLOY-PRIVATE.md` in the uploaded archive for its original deployment notes.

Official references: https://ai.google.dev/gemini-api/docs/aistudio-build-mode
