# Blockchain Concept Visualizer

An interactive, browser-native educational simulator designed for computer science and cybersecurity students to explore, experiment with, and visualize core blockchain mechanisms in real time.

Developed for **CSB24PCC452 (Blockchain)** | **B.Tech Cybersecurity and Blockchain Technology** at **MGM University, Chhatrapati Sambhajinagar**.

---

## 🚀 Live Demo & Quick Start

The visualizer is built entirely in **Vanilla HTML5, CSS3, and JavaScript** with **zero build steps and zero runtime dependencies**.

### Option 1: Direct Browser Launch
Simply open `index.html` or `blockchain_visualizer.html` directly in any modern web browser (Chrome, Edge, Firefox, Brave, Safari).

### Option 2: Local HTTP Server
Run with Python:
```bash
python -m http.server 3000
```
Or with Node / npx:
```bash
npx serve -l 3000 .
```
Then navigate to: **[http://localhost:3000](http://localhost:3000)**

---

## 🧩 Core Interactive Modules

1. **§01 — Hash Lab:**
   * Live SHA-256 computation on every keystroke.
   * Real-time bit entropy calculation and 64-character hexadecimal digest display.
   * **Avalanche Effect Comparator:** Compares the live hash against a reference hash (with 1 character changed) and dynamically calculates the percentage and count of altered bits.

2. **§02 — Block & Chain Builder:**
   * Linked block sequence connected by cryptographic hash pointers ($H_i = \text{SHA-256}(i \parallel \text{timestamp} \parallel \text{data} \parallel H_{i-1} \parallel \text{nonce})$).
   * **Tamper-Evidence Simulation:** Modifying any block's data immediately cascades an invalid state (`.invalid`) downstream, visually showing why modifying past blocks breaks the entire chain.
   * Includes a "Recalculate / Re-Mine" function to demonstrate the computational overhead of re-validating a tampered chain.

3. **§03 — Mining Simulator (Proof of Work):**
   * Satoshi Nakamoto's Proof of Work consensus.
   * Adjustable difficulty targets (1 to 5 leading zeros).
   * Asynchronous nonce-search loop with live Hash Rate (H/s), total attempts counter, and elapsed time tracking.

4. **§04 — Proof of Stake (PoS) Demo:**
   * Models energy-efficient, stake-weighted validator selection.
   * Configurable validator pools (Alice, Bob, Charlie, Dave) with asymmetric stake allocations.
   * Single-round or batch simulation (100 epochs) comparing empirical win frequency against theoretical probability.
   * Interactive **51% Majority Stake Attack** simulator.

5. **§05 — Wallet & Cryptographic Keys:**
   * Visualizes the one-way asymmetric key derivation pipeline:
     $$\text{CSPRNG Seed (256-bit)} \longrightarrow \text{Private Key} \longrightarrow \text{Public Key} \longrightarrow \text{Blockchain Address}$$
   * Demonstrates the trapdoor property of one-way cryptographic functions.

6. **§06 — Transactions & Double-Spend Resolution:**
   * Peer-to-peer balance ledger with digital signature verification.
   * **Double-Spend Race Attack Engine:** Simulates broadcasting conflicting transactions and demonstrates how mempool validation and mined block confirmation depth resolve double spending.

---

## 🎨 Design & Performance

* **Light Academic Theme:** High contrast, legible typography using Google Fonts (`Source Serif 4`, `Inter`, `IBM Plex Mono`).
* **Zero-Lag Native Scrolling:** Fully hardware-accelerated with zero scroll hijacking or continuous background canvas animations, sustaining a smooth 60–120 FPS.

---

## 📁 Repository Structure

```
.
├── .gitignore                                         # Git ignore rules for OS & temporary files
├── index.html                                         # Primary entry point (served at root)
├── blockchain_visualizer.html                         # Standalone single-page application
├── package.json                                       # Project metadata and start scripts
├── package-lock.json                                  # Dependency lockfile
├── README.md                                          # Project overview and instructions
├── PROJECT_CONTEXT.md                                 # Full technical context & guardrails for AI agents
├── Blockchain_Visualizer_Detailed_Project_Report.docx # Comprehensive academic project report (.docx)
└── Blockchain_Visualizer_Project_Report.docx          # Preliminary project report draft (.docx)
```

---

## ☁️ Deployment on Vercel

This project is a static site and can be deployed to Vercel in seconds:

1. Push this repository to your GitHub account.
2. Go to **[vercel.com](https://vercel.com)** and log in with GitHub.
3. Click **"Add New..."** > **"Project"**.
4. Select the **`BlockChain_Simulation`** repository.
5. In **Framework Preset**, select **"Other"** (or leave as default).
6. Root Directory: `./` (leave default).
7. Click **"Deploy"**.
8. Your project will be live at `https://blockchain-simulation-<username>.vercel.app`!

---

## 👨‍💻 Author & Academic Attribution

* **Developer:** Kshitij Pingale
* **Program:** B.Tech, Cybersecurity and Blockchain Technology
* **Institution:** MGM University, Chhatrapati Sambhajinagar
* **Course:** Blockchain (CSB24PCC452)
