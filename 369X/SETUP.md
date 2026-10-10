# 369X setup guide

Takes about an hour. Everything here is free (BNB testnet uses free test BNB).

## What you need
- A GitHub account
- A Netlify account (sign up with GitHub)
- Chrome + the MetaMask extension, with a wallet you control.
  This wallet becomes the **owner of the contracts**. Keep its 12 words safe and never share them.

---

## 1. Put the code on GitHub
1. Unzip the folder.
2. GitHub → **New repository** → name it (for example `369X`) → Create.
3. On the new repo page click **uploading an existing file**, drag in **everything inside** the unzipped folder
   (including the hidden `.github` folder: on Mac press Cmd+Shift+. in Finder to show it), then **Commit changes**.
   Or with git: `git init && git add . && git commit -m "Initial commit" && git branch -M main && git remote add origin <repo url> && git push -u origin main`

## 2. Put the site live on Netlify
1. Netlify → **Add new site → Import an existing project → GitHub** → pick your repo.
2. Build command: leave **empty**. Publish directory: **`.`** (a single dot). Click **Deploy**.
3. You get a link like `something.netlify.app`. Rename it in **Site configuration → Change site name**,
   or add your own domain in **Domain management**.

The site now runs in demo mode (test money in the browser) until you deploy the contracts.
Every commit to `main` redeploys the site automatically.

## 3. Get free test BNB
Open https://www.bnbchain.org/en/testnet-faucet and paste your MetaMask address. You need about 0.3 tBNB for all deploy steps.

## 4. Deploy the contracts (3 rounds)
The deploy page builds the system in three rounds. After each round you paste the new addresses into
`js/config.js`, commit, wait for Netlify to redeploy (1 minute), then open the deploy page again.

**Round 1: tokens + market**
1. Open `https://<your-site>/deploy/` → **Connect MetaMask** (it switches you to BNB Smart Chain Testnet).
2. Keep "Also create the starting markets" ticked → **Deploy** → confirm each MetaMask popup. Keep the page open.
3. Copy the result. In `js/config.js` (edit it on GitHub with the pencil icon) fill in:
   - `CONTRACTS.owner` = your wallet address
   - `CONTRACTS.usdt`, `CONTRACTS.token`, `CONTRACTS.market` from the result
   - `TOKEN_ADDRESS` = the same address as `CONTRACTS.token`
   - set `USE_MOCK: false`
   Commit.

**Round 2: vault + staking.** Open `/deploy/` again → Deploy → copy `vault` and `staking` into `CONTRACTS` → commit.

**Round 3: referral program.** Open `/deploy/` again → Deploy → copy `referral` into `CONTRACTS` → commit.

The deploy page also has a **Verify** panel: run it so anyone can read the contract source on Sourcify.

## 5. Owner settings
Open `https://<your-site>/#/admin` with the owner wallet connected (the menu shows **Safety** and **Analytics** only to the owner).
In **Safety**, set the market creation bond to **5,000** (it starts at 1,000; keep it equal to `CREATE_BOND` in `js/config.js`),
then apply the other recommended settings shown there.

## 6. Fast loading (history snapshot)
1. GitHub repo → **Settings → Actions → General**: allow actions, and under **Workflow permissions** choose **Read and write** → Save.
2. **Actions** tab → **Chain history snapshot** → **Run workflow**.
   It writes `data/chain-snapshot.json`, so visitors don't scan the whole blockchain history on their first visit.
   It runs again every Monday. Each run commits once, which means one Netlify deploy (deploys use Netlify credits on the free plan; you can turn the schedule off in `.github/workflows/chain-snapshot.yml`).

## 7. Your details
In `js/config.js`: `SOCIAL` (your X and Telegram links), `SITE_URL` (your domain). The footer links in `index.html`
(search for "Community") point to the X and Telegram accounts; change them if needed.

## Checklist
- [ ] Site opens, **Connect wallet** works, wallet menu → **Get test funds** gives test USDT and $369X
- [ ] Buy YES on a market and see it in **Portfolio**
- [ ] `#/admin` opens for the owner wallet
- [ ] Actions → snapshot run is green

## Before real money (mainnet)
Contracts need a professional audit, a multisig owner with a timelock, exact-amount token approvals,
and legal advice for the countries you serve. Prediction markets are regulated or banned in many places.
