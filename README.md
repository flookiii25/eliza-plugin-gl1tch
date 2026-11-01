# `eliza-plugin-gl1tch` (v1.2.0)

[![Test Suite](https://img.shields.io/badge/tests-9%2F9%20passed-brightgreen.svg)](https://github.com/flookiii25/eliza-plugin-gl1tch)
[![ElizaOS Plugin](https://img.shields.io/badge/ElizaOS-plugin-blueviolet.svg)](https://elizaos.github.io)
[![Solana Safety](https://img.shields.io/badge/Solana-Token%20Safety-00ff88.svg)](https://gl1tchbased.xyz)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Official **ElizaOS (`@elizaos/core`)** plugin for **GL1TCH**: the multi-vector token safety scanner, pre-trade honeypot firewall, verifiable drand quantum randomness, and Know Your Agent (KYA) trust layer on Solana and EVM.

---

## 🛡️ Why ElizaOS Trading Agents Need This

Autonomous trading agents lose up to 90% of their degen portfolio to honeypots, unlocked LP drains, and unverified creator mints.

`eliza-plugin-gl1tch` provides:
1. **Pre-Trade Guardrail (`gl1tchPreTradeGuard`):** Intercepts buys/swaps before transaction signing. Halts execution if a token is a honeypot or scored below safety thresholds.
2. **Context Provider (`gl1tchSafetyProvider`):** Automatically detects Solana mints or EVM contract addresses in conversation history and injects real-time GL1TCH risk audit data into the LLM context.
3. **Pre-Trade Evaluator (`gl1tchPreTradeEvaluator`):** Evaluates token safety before swapping decisions.
4. **Verifiable drand Quantum Randomness (`drawProvablyFairWinners`):** League of Entropy BLS12-381 threshold beacon for provably fair giveaways, raffles, and gaming agents.
5. **VIP Rug Radar Live SSE Stream:** Sub-second fresh launch screening on Solana.
6. **Know Your Agent (KYA / ERC-8004):** Authenticates and verifies other AI agents' wallet reputations before trading or delegating work.

---

## 📦 Installation

Install directly into your ElizaOS project from GitHub:

```bash
npm install github:flookiii25/eliza-plugin-gl1tch
```

---

## 🚀 Quickstart

### 1. Register with Eliza Character

Add `gl1tchElizaPlugin` to your character's plugin list:

```typescript
import { gl1tchElizaPlugin } from "eliza-plugin-gl1tch";

export const character = {
  name: "Gl1tchTrader",
  modelProvider: "anthropic",
  plugins: [
    gl1tchElizaPlugin({
      // Optional: Pass $GL1TCH Holder key for free priority access
      apiKey: process.env.GL1TCH_API_KEY,
    }),
  ],
};
```

### 2. Pre-Trade Guard (Halt Bad Trades)

Call `gl1tchPreTradeGuard` inside your trade action handler (e.g., before Jupiter / Raydium swap):

```typescript
import { gl1tchPreTradeGuard } from "eliza-plugin-gl1tch";

const mint = "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263"; // e.g. target token

const guard = await gl1tchPreTradeGuard(mint, {
  minScore: 70, // Required safety score (0-100)
  rejectHoneypot: true, // Reject non-transferable tokens immediately
});

if (!guard.approved) {
  console.warn("❌ Trade blocked by GL1TCH Guard:", guard.reason);
  return { success: false, reason: guard.reason };
}

console.log("✅ Safe to trade:", guard.verdict, `(Score: ${guard.score}/100)`);
```

### 3. Provably Fair Community Giveaways & Raffles

Pick verifiable winners from a list of users or wallet addresses using drand BLS12-381 entropy:

```typescript
import { drawProvablyFairWinners } from "eliza-plugin-gl1tch";

const giveaway = await drawProvablyFairWinners({
  participants: [
    "solana_trader_1.sol",
    "degen_whale_2.sol",
    "alpha_bot_3.sol",
  ],
  count: 1, // Number of winners
  salt: "community-raffle-november-2026",
});

console.log("Winner:", giveaway.winners[0]);
console.log("drand Round:", giveaway.round);
console.log("On-Chain Verification:", giveaway.proofUrl);
```

### 4. Know Your Agent (KYA / ERC-8004) Verification

Verify another AI agent's credentials before sending funds or delegating tasks:

```typescript
import { verifyAgentTrust } from "eliza-plugin-gl1tch";

const trust = await verifyAgentTrust({
  address: "AnotherAgentSolanaWalletAddress",
  requiredLevel: "neutral", // "trusted" | "neutral" | "caution"
});

if (trust.isTrusted) {
  console.log("Agent verified! Trust score:", trust.score);
} else {
  console.warn("Unverified or high-risk agent wallet!");
}
```

---

## 🛠️ Actions Included

| Action Name | Description |
|---|---|
| `GL1TCH_SCAN_TOKEN` | Full multi-vector token safety scan (LP lock, honeypot tax, mint authority, insider clusters). |
| `GL1TCH_QUICK_CHECK` | Sub-second lightweight pre-trade screen ($0.001 on x402). |
| `GL1TCH_YIELD_XRAY` | 7-Chain on-chain DeFi yield and risk audit (Aave v3, Kamino, Jito, etc.). |
| `GL1TCH_DEPLOYER_REPUTATION` | Serial rugger check via the GL1TCH Signal Graph. |
| `GL1TCH_RUG_RADAR` | Fresh launch feed sorted riskiest first. |
| `GL1TCH_AGENT_TRUST` | KYA (ERC-8004) trust score for agent wallets. |
| `GL1TCH_VERIFIABLE_RANDOM` | League of Entropy drand verifiable random numbers. |
| `GL1TCH_TOKEN_VERDICT` | Compact verdict card with shareable image link. |

---

## 🧪 Testing

Run native tests (no dependencies required):

```bash
npm test
```

---

## 📜 License

MIT © [GL1TCH](https://gl1tchbased.xyz)
