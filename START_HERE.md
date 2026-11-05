# Sutra

Sutra is Osiris Medici's workspace for improving the UI and behavior of [VedAnvaya](https://github.com/HimanshuMohanty-Git24/VedAnvaya). The initial application and corpus work are by the upstream contributors. Their history, license, notices, and documentation are retained. The app still displays its original VedAnvaya branding.

Repository: https://github.com/OsirisMedici/Sutra

## Start development

```sh
cd frontend
pnpm install --frozen-lockfile
cp .env.example .env.local
pnpm dev --hostname 127.0.0.1 --port 5312
```

The T3 project is named Sutra and has Develop, Build, and Typecheck actions. Its VPS checkout is `~/projects/Sutra`.

The current frontend uses the upstream public API at `https://vedagraph-api.onrender.com`. This keeps the VPS lightweight and requires no local database or paid account. Corpus data, uptime, and Ask availability depend on that external service. Do not assume it is an owned or guaranteed backend. No paid AI request was made during setup.

For a self-contained backend, follow LOCAL_SETUP.md. The repository excludes the populated corpus database and dump; an empty Neo4j installation will not make corpus browsing work.

## Public access without Tailscale

Anyone with the public app link can open Sutra in a browser without Tailscale or an account.

Requests follow this path:

```text
Visitor -> Cloudflare public HTTPS link -> outgoing tunnel to the VPS -> Sutra
```

The VPS maintains an outgoing connection to Cloudflare. Cloudflare forwards public requests through that connection to the running app. Visitors do not need direct network access to the VPS. The tunnel exposes the app, not the private Tailscale network.

This uses a free Cloudflare Quick Tunnel. The URL changes if the tunnel process restarts, and the app must remain running on the VPS. A permanent domain with a named tunnel would provide a stable address. The existing Tailscale link is a separate way to reach the same app.

## Current deployment

The VPS serves the production build under the user service `sutra-web`. A free Cloudflare Quick Tunnel provides public HTTPS through `sutra-public`.

Current public link: https://swim-shopzilla-dns-authentication.trycloudflare.com

The tunnel URL changes if its process restarts. It is a temporary sharing link, not a permanent domain. Its current address is recorded in the `sutra-public` journal. The app also remains reachable over Tailscale on port 5311.

```sh
systemctl --user status sutra-web sutra-public
journalctl --user -u sutra-web -u sutra-public
```

The service reads this checkout directly. Stop it before rebuilding, then start it again:

```sh
systemctl --user stop sutra-web
cd frontend
pnpm build
systemctl --user start sutra-web
```

The production service currently reads this checkout's `.next` build, so coordinate rebuilds with readers or use a separate release directory for future zero-downtime deployments. Do not restart the tunnel just to deploy a frontend change.

## Next development session

Start with the user's chosen UI or interaction improvement. No redesign has been implemented yet. Review the existing components and product-motion skill when changing interactive behavior. Validate the affected user flow, not only the build. Preserve upstream attribution and the existing software and data license terms.

Useful checks from `frontend/` are `pnpm typecheck`, `pnpm lint`, `pnpm test`, and `pnpm build`. Use checks appropriate to the change. The original README below and LOCAL_SETUP.md document the full product.
