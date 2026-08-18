# Gas-in-USDC — a tiny Arc testnet UX demo

**Live demo: https://leedlgusgn.github.io/arc-gas-in-usdc/**

A single-file web app that makes one Arc property legible to a normal person:
**on Arc, USDC is the native gas token**, so the fee and the amount you send are
denominated in the same unit.

There is a **read-only lookup** on the page — paste any Arc testnet address and it shows
the balance and the current transfer fee without connecting a wallet.

Most wallet UIs were designed for chains where you hold a volatile asset just to
pay fees. That produces a familiar failure: a user has the money they want to
send, but not the *other* token needed to send it. Arc removes that second asset —
this demo is a small check on whether the UI can actually show that clearly.

## What it does

- Adds / switches to Arc Testnet (chain `5042002`) via `wallet_addEthereumChain`
- Reads your native balance and labels it as what it is: USDC that is also your gas budget
- Estimates a transfer and shows **amount / fee / total in USDC** side by side
- Sends the transfer and reports the actual fee paid from the receipt, with an explorer link

## Run it

No build step, no dependencies to install.

```bash
# any static server, or just open the file
npx serve .
```

Then get testnet USDC from https://faucet.circle.com and connect a wallet.

> Testnet only. Use a throwaway wallet — never one holding real funds.

## Network details used

| | |
|---|---|
| Chain ID | 5042002 (`0x4CEF52`) |
| RPC | https://rpc.testnet.arc.io |
| Native currency | USDC (18 decimals) |
| Explorer | https://testnet.arcscan.app |

## Stack

`ethers v6` from CDN, vanilla JS, one HTML file (~250 lines). Deliberately minimal
so the interaction is the subject, not the framework.

## Design notes / open questions

Things I ran into that seem worth discussing with other Arc builders:

1. **Fee legibility.** Showing the fee in USDC makes cost obvious, but the numbers are
   tiny. Measured on testnet (block 57,596,714): a plain transfer costs
   **21,000 gas × 21 gwei = 0.000441 USDC**. At 6 dp that reads cleanly as `0.000441`;
   at 2 dp it renders `0.00`, which users read as *broken* rather than *cheap*.
   Where should the cut be — and should the UI ever say "less than a hundredth of a cent"
   in words instead?
2. **"Total leaves wallet"** is the number users actually care about, yet most wallet
   confirmation screens still separate value and fee. Worth a pattern.
3. **Balance framing.** With one asset for value *and* gas, "available to send" is not
   the full balance — you must reserve the fee. Reserving it silently confuses people;
   showing it as a line item is clearer.

Feedback welcome — especially from anyone who has shipped consumer payment flows.

---

Built by a product designer (UX/UI at Verse8, Planetarium) who codes the front end.
