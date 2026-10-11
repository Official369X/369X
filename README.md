# 369X

On-chain prediction market on BNB Smart Chain.
Plain HTML/CSS/JS with **no build step**: open it, edit it, deploy it.

## Features
| Page | What it does |
|---|---|
| **Markets** | Browse, search, filter by category/status, sort by volume/trending/new/ending |
| **Market page** | Live price chart, LMSR pricing, buy YES/NO, **1×–10× leverage**, liquidation price, activity feed, rules |
| **Create market** | Anyone posts a question with a $369X bond and earns 0.5% of every trade |
| **Portfolio** | Balances, open positions (close any time), P&L, history, markets you created |
| **Vault** | Deposit USDT that backs leverage. Flex / 90 / 180 / 365-day locks = 1× / 2× / 4× / 8× points |
| **Stake** | Stake $369X for fee discounts (up to 50%) and voting power |
| **Resolution** | Stakers vote on ended markets; correct voters earn rewards; positions settle automatically |
| **Rewards** | Points from every activity + 10 badges |
| **Leaderboard** | By profit, volume or points |
| **Affiliate** | Referral links, tiers up to 35%, claim earnings |
| **Token / Docs** | Tokenomics, utilities, full how-it-works guide |
| **Faucet** | Free test USDT + $369X every 24h (in the wallet menu) |

Wallets: MetaMask, Trust Wallet, Coinbase Wallet, Rabby and Binance Wallet all work and are switched to BSC Testnet automatically. With no wallet app installed, the site uses a demo wallet.

## Files
```
index.html        page layout (header, footer, menus)
css/styles.css    all styling
js/config.js      ⭐ THE ONLY FILE YOU NEED TO EDIT: names, fees, links, network, tiers
js/lmsr.js        price math (LMSR market maker)
js/api.js         data layer + demo engine (swap for real backend/contracts later)
js/wallet.js      wallet connection
js/app.js         all pages and buttons
assets/logo.png   your logo
```

## Run it on your computer
Double-clicking `index.html` works for a quick look. For a wallet to connect, use a local server instead:
```
python3 -m http.server 8000
```
then open http://localhost:8000

## Deploy
Push to GitHub, import the repo at vercel.com/new, and click Deploy. There are no settings to change.

## Status
The site runs **on-chain on BSC Testnet** (`CHAIN_ON: true` in `js/config.js`): markets, trading, the faucet, vault, staking and referrals all use the smart contracts listed under `CONTRACTS`.
Set `CHAIN_ON: false` and `USE_MOCK: true` for a browser-only demo with no contracts.
Before real funds: audited mainnet contracts and a legal review.

## Deploy the contracts
Open `/deploy/` on the live site with MetaMask on BSC Testnet (get free test BNB from the faucet link on that page). It runs in three rounds; after each round, copy the addresses it shows into `CONTRACTS` in `js/config.js`, publish, and open `/deploy/` again:
1. Core: test USDT, test $369X, the market contract and the starting markets. Copy `owner`, `usdt`, `token`, `market` (and set `TOKEN_ADDRESS` to the `token` address).
2. Vault + staking. Copy `vault`, `staking`.
3. Referral program. Copy `referral`.

The wallet that runs round 1 owns the contracts and sees the **Safety panel** and **Analytics** pages.

## Smart contracts (BNB Smart Chain Testnet)
`contracts/` holds the Solidity contracts (Hardhat project):
- `TestToken.sol`: test USDT and test $369X with a 24-hour faucet
- `Market369X.sol`: LMSR YES/NO markets: create (with $369X bond), buy, sell, resolve, redeem, creator fees

```
cd contracts && npm install && npm test     # 12 tests incl. a 300-trade solvency stress test
npm run build                               # recompile + refresh deploy/artifacts.json
```
Deploy from the browser with MetaMask at `/deploy/` (no private keys needed).
