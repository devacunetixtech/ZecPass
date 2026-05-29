# ZecPass

> Privacy-Preserving Sign-In & Identity Layer for Zcash

**ZecHub 2026 Hackathon — Track 4: Zcash Login**

ZecPass lets any web app add "Sign in with Zcash" — users prove ownership of a shielded Zcash address by sending a signed memo, without ever exposing their address to the app or any third party. A ZK proof hash (app-scoped SHA256 derived value) serves as the stable, address-free user identifier.

## ✨ Features

- **🔒 Zero Address Exposure** — the authenticating app never learns the user's Zcash address
- **🛡️ Replay-Proof** — every challenge is single-use, time-bound, and cryptographically tied
- **🔌 Drop-in SDK** — `<ZecPassButton />` React component + Node.js verification library
- **🏅 ZK Identity Badges** — privacy-preserving credentials and achievements
- **⚡ Real-time** — SSE-based challenge status updates

## 🚀 Quick Start

```bash
# 1. Clone and install
git clone https://github.com/zecpass/zecpass.git
cd zecpass
pnpm install

# 2. Set up environment
cp .env.example .env
# Edit .env with your MongoDB URI and generate JWT keys:
# openssl genrsa -out private.pem 2048
# openssl rsa -in private.pem -pubout -out public.pem
# Base64 encode and set JWT_SECRET and JWT_PUBLIC_KEY

# 3. Run development servers
pnpm dev

# 4. Open http://localhost:3000
```

## 📁 Project Structure

```
zecpass/
├── apps/web/              # Next.js 15 — frontend + API routes
├── packages/sdk-react/    # <ZecPassButton /> React component
├── packages/sdk-node/     # ZecPassClient Node.js library
├── zingolib-service/      # Hono server wrapping ZingoLib
├── docker-compose.yml     # Full stack deployment
└── ARCHITECTURE.md        # Detailed architecture docs
```

## 🔐 How It Works

1. **App requests challenge** → ZecPass issues a shielded address + nonce
2. **User sends memo** → Zcash shielded memo with `ZECPASS:v1:{challenge}:{nonce}:{timestamp}`
3. **ZecPass verifies** → Checks format, nonce, timing, replay protection
4. **App receives JWT** → Contains `zk_proof_hash = SHA256(challenge:tx:nonce:app)` — never the address

## 🧩 SDK Usage

### React

```tsx
import { ZecPassProvider, ZecPassButton } from '@zecpass/sdk-react';

<ZecPassProvider appId="your-app-id" redirectUri="/auth/callback">
  <ZecPassButton onSuccess={(session) => console.log(session.zk_proof_hash)} />
</ZecPassProvider>
```

### Node.js

```typescript
import { ZecPassClient } from '@zecpass/sdk-node';

const zecpass = new ZecPassClient({ appId: '...', appSecret: '...' });
const session = await zecpass.verifyToken(token);
```

## 📖 Documentation

- [Architecture](./ARCHITECTURE.md) — System design, protocol, database schema
- [Web App](./apps/web/README.md) — Environment setup, running, testing
- [React SDK](./packages/sdk-react/README.md) — Component API reference
- [Node.js SDK](./packages/sdk-node/README.md) — Client API reference

## 🧪 Testing

```bash
pnpm -F @zecpass/web test        # Unit + integration tests
```

## 🐳 Docker (Optional)

```bash
docker-compose up
```

## 📄 License

MIT — ZecPass is open-source and privacy-first.

---

*Built for ZecHub 2026 Hackathon. Track 4: Zcash Login.*
