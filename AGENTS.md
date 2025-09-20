# Repository Guidelines

## Project Structure & Module Organization
- `server/` hosts the Node.js entrypoint (`server/index.js`) and WebSocket logic; keep backend updates modular.
- `public/` contains the static SPA with `scripts/` for client logic, `styles/`, `lang/` translations, and the `service-worker.js` cache handler.
- `pairdrop-cli/` bundles the optional CLI and OS shell integrations; mirror changes across shell scripts when modifying.
- `dev/` holds Docker/TLS helpers, while `docs/` and `licenses/` maintain handbooks and third-party notices; workflows live in `.github/workflows/`.

## Build, Test, and Development Commands
- `npm install` — install backend dependencies (Node >= 15 recommended via `nvm use`).
- `npm start` — launch the local server on port 3000 for quick smoke tests.
- `npm run start:prod -- --rate-limit` — production-like start with rate limiting and auto-restart flags.
- `docker compose -f docker-compose-dev.yml up --build` — run backend plus TLS proxy defined in `dev/`; certs land in `dev/certs`.
- `docker compose up -d` — bring up the production stack from `docker-compose.yml`.

## Coding Style & Naming Conventions
- ES modules, 4-space indentation, and existing semicolon usage; align with surrounding code rather than reformatting wholesale.
- Use descriptive camelCase for functions and vars, PascalCase for classes, constants in UPPER_SNAKE.
- Store UI copy in `public/lang/*.json`; reuse keys to keep translations in sync.

## Testing Guidelines
- No automated suite yet: validate peer discovery, file transfer, and room pairing manually on at least two devices/browsers.
- For CLI tweaks, run `pairdrop-cli/pairdrop` and platform launchers (`send-with-pairdrop.ps1`, `.sh`) in their target environments.
- Record manual steps and regressions checked in your PR description; attach screenshots for UI adjustments.

## Commit & Pull Request Guidelines
- Follow short sentence-case commit subjects (e.g., `Only handle requests with the same origin via the service worker`); group refactors separately from features.
- Reference issues with `Fixes #123` or `Refs #123`; leave dependency bumps to Dependabot unless coordinated.
- PRs should explain intent, list manual verification, and add before/after visuals for front-end work; wait for Docker build workflows under `.github/workflows/` to succeed.

## Security & Configuration Tips
- Keep secrets out of version control; base configs on `rtc_config_example.json` and `turnserver_example.conf` and load real values via env vars.
- When enabling TURN/STUN, point `RTC_CONFIG` to secure paths and rotate credentials if shared outside your network.
