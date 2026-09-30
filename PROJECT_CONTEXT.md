# Blockchain Concept Visualizer — Comprehensive AI Agent Context Document

> **Target Audience:** AI Coding Agents (Claude, Gemini, GPT-4, Cursor, Copilot, Antigravity, etc.)  
> **Repository Purpose:** Educational, client-side interactive blockchain simulator built for undergraduate computer science students.  
> **Academic Context:** Course **CSB24PCC452** (Blockchain, 2 Credits), B.Tech Cybersecurity & Blockchain Technology, MGM University, Chhatrapati Sambhajinagar.  
> **Primary Author:** Kshitij Pingale.

---

## 1. Executive Project Summary

The **Blockchain Concept Visualizer** is a zero-dependency, browser-native educational tool. It translates abstract, one-way-function-heavy mathematical concepts (cryptographic hashing, chained tamper-evidence, Proof-of-Work mining, Proof-of-Stake consensus, asymmetric key derivation, and double-spending race conditions) into direct, hands-on interactive simulations.

### Core Architectural Philosophy
* **Zero Runtime Dependencies:** Everything runs directly in modern browsers without Node.js runtime overhead, npm packages, or compilation steps.
* **Pedagogical Transparency:** All cryptographic algorithms (specifically RFC 6234 compliant SHA-256) and consensus logic are written in synchronous, human-readable vanilla JavaScript so students can inspect the exact math in browser DevTools.
* **Lightweight & High-Performance:** Engineered with native browser scrolling and pure CSS transitions. All heavy animation libraries (Lenis scroll hijacking, continuous 2D background canvas loops, 3D mousemove card-tilts) have been stripped to maintain a locked 60–120 FPS.

---

## 2. Repository Layout & File Manifest

```
d:/projects/blockchain/
├── blockchain_visualizer.html                         # [PRIMARY ACTIVE APP] Monolithic, light-theme visualizer (1,867 lines)
├── index.html                                         # Mirror of blockchain_visualizer.html (served at root)
├── package.json                                       # Project metadata and execution scripts
├── package-lock.json                                  # Lockfile for reproducible local tooling
├── Blockchain_Visualizer_Detailed_Project_Report.docx # Full 45KB academic/technical project report (.docx)
├── Blockchain_Visualizer_Project_Report.docx          # Earlier 11KB preliminary report draft (preserved)
├── PROJECT_CONTEXT.md                                 # [THIS FILE] Context & architectural guide for AI agents
└── .gitignore                                         # Git ignore rules for OS/editor metadata and dependencies
```

---

## 3. Tech Stack & Environment

| Layer | Technology | Key Details |
| :--- | :--- | :--- |
| **Markup** | HTML5 Semantic | `<header>`, `<nav id="chain-thread">`, `<main>`, `<section class="module-panel">` |
| **Styling** | Vanilla CSS3 | Standard CSS custom properties (`:root`), Flexbox, CSS Grid. **No Tailwind**. |
| **Fonts** | Google Fonts | `Source Serif 4` (Headings), `Inter` (UI/Labels), `IBM Plex Mono` (Hashes/Code) |
| **Scripting** | Vanilla JavaScript | Pure ES6+ closures; zero external runtime imports. |
| **Crypto** | In-house SHA-256 | Pure JS implementation of NIST FIPS 180-4 / RFC 6234 (64 rounds, bitwise 32-bit ops). |
| **Entropy** | `window.crypto` | Uses `crypto.getRandomValues()` for CSPRNG 256-bit key seeds. |
| **Dev Server**| Python / NPX | `python -m http.server 3000` or `npx serve -l 3000 .` |

---

## 4. UI Design System & Theme Specification

The visualizer uses a **Light Professional Academic Theme** (refactored from an unreadable dark/neon theme). When adding UI or modifying CSS, adhere strictly to these CSS variables defined in `:root`:

```css
:root {
  --paper:        #f8f9fc;               /* Main background canvas */
  --ink:          #1e293b;               /* Primary typography (slate dark) */
  --ink-dim:      #64748b;               /* Secondary/muted typography */
  --accent:       #3b82f6;               /* Primary interactive blue */
  --accent-light: #60a5fa;
  --accent-faint: rgba(59,130,246,0.08); /* Highlight tags / chips */
  --accent-hover: #2563eb;
  --gold:         #d97706;               /* Mining / nonce / reward indicator */
  --gold-faint:   rgba(217,119,6,0.08);
  --green:        #059669;               /* Valid chain state / success */
  --green-faint:  rgba(5,150,105,0.06);
  --red:          #dc2626;               /* Invalid chain / double-spend attack */
  --red-faint:    rgba(220,38,38,0.06);
  --border:       #e2e8f0;               /* Clean card borders */
  --border-hover: #cbd5e1;
  --card-bg:      #ffffff;               /* Pure white container cards */
  --shadow-sm:    0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow:       0 4px 16px rgba(0, 0, 0, 0.06);
  --shadow-lg:    0 8px 32px rgba(0, 0, 0, 0.08);
  --radius:       10px;
  --radius-lg:    14px;
  --transition:   200ms ease;
}
```

---

## 5. Detailed Module-by-Module Technical Logic

The visualizer contains 6 distinct modules organized in tabbed `<section>` elements:

### Module 01: Hash Lab (`id="m01"`)
* **Core Concept:** SHA-256 determinism, fixed output length, and the **Avalanche Effect**.
* **Key Elements:**
  * `#hash-input`: Text input listening to the `input` event.
  * `#hash-output`: Real-time 64-character hexadecimal digest.
  * **Avalanche Comparator:** Compares the live hash against a reference hash (input string modified by 1 character). Computes bitwise and nibble-wise differences and outputs the percentage of changed characters.
* **Implementation:** Pure JS `sha256(input)` called synchronously.

### Module 02: Block & Chain Builder (`id="m02"`)
* **Core Concept:** Chained block structure, hash pointers, and tamper-evident cascading invalidation.
* **Data Model:**
  * Global `chain` array storing block objects: `{ index, timestamp, data, previousHash, nonce, hash }`.
  * Block #0 is Genesis: `previousHash = "0000000000000000000000000000000000000000000000000000000000000000"`.
* **Tampering Simulation:**
  * When a user edits block $k$'s data, the chain validator recalculates:
    $$H_i = \text{SHA-256}(i + \text{timestamp} + \text{data} + H_{i-1} + \text{nonce})$$
  * Because $H_k$ changes, Block $k+1$'s `previousHash` no longer matches $H_k$.
  * State cascade immediately marks blocks $k, k+1, \dots, n$ with CSS class `.invalid` (red border & alert).
  * A "Recalculate / Re-Mine" button demonstrates the computational cost of recalculating valid hashes downstream.

### Module 03: Mining Simulator (Proof of Work) (`id="m03"`)
* **Core Concept:** Satoshi Nakamoto's Proof of Work and adjustable difficulty target.
* **Target Requirement:** Hash must start with $D$ leading zeros (where difficulty $D \in [1, 5]$).
* **Asynchronous Nonce Loop:**
  * Uses `requestAnimationFrame` (or chunked `setTimeout`) so the browser does not hang during intensive hashing.
  * Increments `nonce = 0, 1, 2, ...` checking `sha256(data + nonce).startsWith(target)`.
  * Metrics dashboard shows: Real-time Nonce, Hash Rate (H/s), Elapsed Time, and Rolling Hash.

### Module 04: Proof of Stake (PoS) Demo (`id="m04"`)
* **Core Concept:** Energy-efficient, stake-weighted random block validation.
* **Data Model:**
  * Array of validators with token balances (e.g. Alice: 40 ETH, Bob: 25 ETH, Charlie: 20 ETH, Dave: 15 ETH).
* **Consensus Selection:**
  * Implements cumulative stake-weighted selection:
    $$P(\text{Validator } i) = \frac{\text{Stake}_i}{\sum \text{Stake}}$$
  * Can run single round epochs or batch simulation of 100 rounds.
  * Tracks empirical vs. theoretical block proposal percentages and includes a **51% Attack** simulator.

### Module 05: Wallet & Cryptographic Keys (`id="m05"`)
* **Core Concept:** Asymmetric key generation and one-way address derivation hierarchy.
* **Pipeline:**
  $$\text{Entropy Seed (CSPRNG)} \rightarrow \text{Private Key (256-bit)} \rightarrow \text{Public Key} \rightarrow \text{Wallet Address}$$
* **Important Pedagogical Note:** Key generation uses `window.crypto.getRandomValues()` + SHA-256 transformations as an educational stand-in for Bitcoin's `secp256k1` ECDSA. The UI explicitly alerts students to this simplification.

### Module 06: Transactions & Double-Spend Resolution (`id="m06"`)
* **Core Concept:** The double-spend problem and how block inclusion resolves unconfirmed race conditions.
* **Features:**
  * Account balances ledger, digital signature generation, and peer-to-peer transfers.
  * **Double-Spend Attack Mode:**
    * Simulates sending the same funds concurrently to Merchant A and Attacker's own address.
    * Explains the role of the mempool vs. mined block confirmations in preventing race attacks.

---

## 6. Critical Guardrails for Future AI Agents

When modifying or expanding this codebase, any AI agent **MUST** follow these rules:

1. **DO NOT introduce third-party runtime frameworks:** Do NOT rewrite this into React, Vue, Next.js, or Svelte. It must remain a pure, zero-build HTML/CSS/JS application runnable by opening the file directly.
2. **DO NOT reintroduce scroll hijacking or heavy canvas loops:**
   * Never re-add `Lenis` smooth scroll (it hijacks wheel events and causes severe lag on Windows).
   * Do not attach unthrottled `mousemove` listeners with 3D matrix transforms across cards.
   * Do not run continuous `requestAnimationFrame` canvas particle loops in the background.
3. **Always preserve the Light Academic Theme:**
   * Keep contrast high (WCAG AAA).
   * Background: `#f8f9fc`, Cards: `#ffffff`, Text: `#1e293b`, Accent: `#3b82f6`.
4. **Synchronize `index.html`:**
   * Whenever modifying `blockchain_visualizer.html`, always copy the updated code to `index.html` so `http://localhost:3000/` stays updated.
5. **Preserve Educational Clarity:**
   * Code in `blockchain_visualizer.html` must remain human-readable with descriptive comments so university students can read it directly.
6. **Input Sanitization:**
   * Always pass dynamic string injections through the built-in `escapeHTML()` helper to prevent XSS.

---

## 7. Useful Terminal & Execution Commands

```powershell
# 1. Run local development server via Python (Port 3000)
python -m http.server 3000

# 2. Or run via npx serve
npx serve -l 3000 .

# 3. Synchronize visualizer changes to index.html
Copy-Item blockchain_visualizer.html index.html -Force

# 4. Check JavaScript syntax without running
node -e "const fs = require('fs'); const c = fs.readFileSync('blockchain_visualizer.html', 'utf8'); const s = c.match(/<script>([\s\S]*?)<\/script>/)[1]; eval('try{' + s + '}catch(e){console.error(e)}'); console.log('Syntax check passed');"
```
