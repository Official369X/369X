# 369X

On-chain prediction market on BNB Smart Chain. YES/NO markets priced by an LMSR market maker,
with a USDT vault, $369X staking and resolution voting, referrals, points and badges.

Plain HTML/CSS/JS with **no build step**. Smart contracts are in `contracts/` (Hardhat).

**First time? Follow `SETUP.md`.**

## Files
```
index.html               page layout (header, footer, menus)
css/styles.css           all styling
js/config.js             THE file you edit: names, links, fees, contract addresses
js/app.js                all pages and buttons
js/api-chain.js          reads/writes the smart contracts
js/chain-events.js       contract events (shared with the snapshot script)
deploy/                  browser page that deploys the contracts with MetaMask (/deploy/)
contracts/               Solidity contracts + tests
scripts/chain-snapshot.js  writes data/chain-snapshot.json (fast first load)
.github/workflows/       weekly snapshot refresh
marketing/               X posts, banners, videos, profile headers
netlify.toml / vercel.json  hosting settings (security headers, caching)
```

## Run locally
```
python3 -m http.server 8000
```
Open http://localhost:8000. A wallet only connects over http://localhost or https.

## Contracts
```
cd contracts && npm install && npm test
npm run build          # recompile and refresh deploy/artifacts.json
```
