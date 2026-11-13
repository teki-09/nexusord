# NEXUSORD — Interactive Music Provenance & Piracy-Response MVP

**An interactive, browser-only product demonstration covering music inscriptions, provenance, licensing, BRC-20, simulated Bitcoin indexing, AI-assisted piracy review, and DRM policy decisions.**

**live demo : https://teki-09.github.io/nexusord/**

> **Demo, not a blockchain service.** The interface runs on JavaScript and JSON in one `index.html`. It demonstrates connected workflows and observable state changes, but it does not inscribe on Bitcoin, operate an actual BRC-20 wallet, verify legal copyright ownership, scan the internet with AI, or enforce production DRM.

## Launch the demonstration

**Live site:** If this repository is published with GitHub Pages, open its Pages URL.

**Local preview:** Open `index.html` in a modern browser. For the most consistent Web Crypto and file-handling support, serve it from `localhost` or GitHub Pages (HTTPS):

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000/`. No npm install, dependencies, backend, API keys, accounts, database, images, or external CSS/JS files are required.

### Deploy to GitHub Pages

1. Create a GitHub repository and upload **`index.html`**, **`README.md`**, and **`WALKTHROUGH.txt`** to the repository root.
2. Commit the files to the `main` branch.
3. Open **Settings → Pages → Build and deployment**.
4. Select **Deploy from a branch**, choose **`main`** and **`/ (root)`**, and save.
5. Open the GitHub Pages URL shown in your repository settings.

The application is a static site; GitHub Pages runs the same `index.html` with no build step. Visitor data stays in that visitor's own browser rather than being shared among visitors.

## The product idea

NEXUSORD proposes a common evidence backbone for recorded music and its permissions:

```text
Artist / Rights Claimant
   │
   ▼
Music file + track details + exact licence wording + rights scope
   │
   ├── Real SHA-256 of selected file bytes (or clearly marked metadata fallback)
   └── Locally generated, signed claim manifest
   │
   ▼
Simulated Bitcoin Ordinals commit → reveal → block → indexer
   │
   ├── Music Assets: searchable track and reference ID
   ├── Provenance: hash, claim, signature, lineage and evidence export
   ├── Licences & Disputes: permissions, corrections and human review
   └── Node & Events: pending operations and audit trail
   │
   ├── BRC-20 demo: SEPARATE fungible token ledger, not copyright ownership
   │
   ▼
AI & DRM Lab (future integrations, presently simulated)
   ├── Suspected piracy pattern → case → evidence → human review
   └── Requested use + permission/dispute rules → ALLOW / DENY
```

**Critical principle:** a matching file digest demonstrates byte identity; a valid signature shows that a particular key signed a claim; an inscription reference illustrates timestamped anchoring; a licence or rights determination is a separate question. None of those facts, on its own, proves legal ownership or authorizes a use. A BRC-20 balance never automatically conveys music rights.

## What's in the app

| Screen | What you can do | What visibly changes |
|---|---|---|
| **Overview** | See cross-module KPIs; run a connected example; navigate the six-stage tour; reset | Indexed tracks, pending jobs, token tickers, piracy cases, and recent events |
| **Inscribe Studio** | Add title/claimant, claimed rights scope, licence category, exact licence wording, support reference, and optional audio file | New signed claim, fingerprint, queued commit/reveal jobs, updated track catalog |
| **Music Assets** | Search tracks/artists/IDs; open asset details; export a record or evidence; request a local preview | Status, synthetic inscription ID, mock tx references, SHA-256 basis, version and claim details |
| **Provenance & Rights** | Look up an inscription ID; compare optional digest or reselected file; inspect provenance rows | Match/mismatch, manifest signature/licence-hash check, reference state, audit event |
| **BRC-20 Ledger** | Deploy a mock four-character ticker, mint within limits, prepare and send a transfer | Queued/mined operations, supply, balances, reserved amounts, transfer state |
| **Licences & Disputes** | Grant permissions; open/close a dispute; append an amended claim; export and verify a portable evidence package | Permission ledger, review queue, new version linked to old version, signature verification |
| **AI & DRM Lab** | Run scenario-based suspected-misuse checks, compare exact file hashes, create/resolve incidents, evaluate access rules | Mock incident scores, evidence status, permission ALLOW/DENY decisions, events |
| **Node & Events** | Mine mock blocks; pause/resume/catch up the indexer; simulate a last-block reorganization; inspect events | Chain height, indexer height, pending/indexed states, event console |
| **How It Works** | Follow the eight-step presenter sequence; inspect architecture and JSON; export/import; run consistency checks | Full-state JSON snapshot, validation results, workflow navigation |

`Music Assets → Details` is a detail page rather than a separate main navigation button.

## Five-minute demo: watch a single track propagate

1. Open **Overview → Run a complete demonstration**. The program creates one tutorial record and simulates claim signing, two confirmation steps, a licence grant, access decision, AI incident, and dispute.
2. Visit **Music Assets** and open the newly created **Studio Session · Demonstration …** track. Inspect its synthetic inscription ID, status, digest, claim scope, signature, licence wording, and version.
3. Open **Provenance & Rights**. Select the record and choose **Run local verification**. The record should be found in the local index; its generated signed manifest and preserved licence can be checked. A digest match is assessed only if you provide a digest or file.
4. Open **Licences & Disputes**. Find the permission grant and open dispute generated by the demo. Try an additional grant, append a correction, or export the selected evidence package.
5. Open **AI & DRM Lab**. See the simulated incident, create its evidence, resolve it, and compare permission decisions for different roles/actions. An unresolved dispute restricts sensitive actions in the demo rules.
6. Open **Node & Events** to read the resulting event timeline. The chain, indexer, and transaction identifiers shown are **simulations**.
7. Open **How It Works** for the eight clickable presentation steps and the JSON/diagnostics controls.

For a more deliberate live demonstration, use the detailed `WALKTHROUGH.txt` in this repository.

## Workflows and important behavior

### 1. Music registration and claim signing

- Enter a **track title**, **claimant/artist**, **licence category**, and **exact licence text**. Select whether the claim covers the **master recording**, the **composition**, or **both**. A supporting reference is optional.
- With an audio file selected, the browser computes its actual **SHA-256 byte digest** via Web Crypto. Files are limited to **30 MiB in this demo** for browser memory safety. The file itself is not uploaded or saved to local storage.
- Without a file, the app hashes a metadata object and labels the result **metadata-only placeholder**. It is **not** an audio fingerprint.
- The browser creates a structured rights manifest (including content hash, hash basis, rights scope, licence hash, version, and previous record ID if applicable), then signs it with a **new locally generated ECDSA P-256 key pair**. The exported public key and signature allow integrity checking. This is **not** a verified identity, PKI trust chain, or ownership certificate.
- The app produces synthetic transaction hashes and an Ordinals-*shaped* inscription ID (`<64 hex chars>i0`). The reference is not an actual Bitcoin inscription.

**Automatic mine ON:** submission simulates the commit and reveal block transitions and indexes the result. **Automatic mine OFF:** the first manual mine confirms the commit and queues the reveal; the second mine confirms the reveal (unless the demo indexer is paused).

### 2. Assets and provenance

An indexed claim appears in the asset catalog and provenance table. Search by track, claimant, or ID, then open **Details** to inspect the claim and mock chain references. **Verify this inscription** opens the provenance screen with the chosen ID. A verification lookup can test:

- Whether the ID is present in the **local** reference index.
- Whether a provided expected SHA-256 digest, or a reselected original file's real SHA-256, matches the stored value.
- Whether an **app-created** signed manifest, licence-text hash, and ECDSA signature pass integrity verification.

The three seeded records have **illustrative digests and no source audio/signature**; avoid describing them as validated original recordings. Even a positive integrity result is **not legal ownership verification**.

### 3. Licences, corrections, disputes and evidence

- An explicit grant links an asset, a recipient role/identity, and an allowed action (`preview`, `play`, `download`, or `remix`).
- A dispute is preserved as a review item instead of deleting the earlier claim. Closing it records a human-review **simulation**.
- A correction generates a **new signed version**, with a `parentId` pointing to the earlier record. The previous record remains inspectable; the corrected version has its own simulated inscription lifecycle.
- **Evidence package** export downloads a JSON packet containing the record reference, signed manifest, licence text, public verification key, signature, and scenario/chain limitations.
- **Verify an evidence package without an index** lets the same browser app check a previously exported packet's internal cryptographic integrity. It does **not** contact Bitcoin or establish the signer’s real-world authority.

### 4. BRC-20 ledger (intentionally separate)

The seeded tickers are `AUDI` and `NXRD`. In the simulated ledger you can:

1. **Deploy** a new **four-character alphanumeric ticker** with max supply and per-mint limit; then mine a mock block.
2. **Mint** an amount within supply/limit constraints; then mine a block.
3. **Prepare transfer** to a demo wallet label; mine to make it ready; select **Send**; mine again to settle and update balances.

Token operations use shared pending jobs, block height, and node events. The app checks basic max-supply and balance constraints. It is **not** a consensus-complete BRC-20 indexer, token marketplace, wallet, payment rail, or a mechanism for transferring copyright.

### 5. AI suspected-piracy module — simulated integration

Select a reference track and a scenario: **exact reupload**, **altered/transcoded version**, **suspected unlicensed commercial use**, or **likely authorized/low-risk use**. The application creates a case with a **preset demonstrative score**; this number is not a trained model's confidence. Optionally select a suspect file to run a real **byte-for-byte SHA-256 comparison** against the stored reference digest. Different bytes can disprove an *exact* hash match, but do not prove that altered audio is unrelated.

A flagged case can move to **Create evidence** and **Resolve**. Resolution means the simulated workflow was reviewed; it does **not** send takedowns or automatically punish users. There is no crawling, audio matching model, or source attribution service.

### 6. DRM decision-point — simulated integration

The demo evaluates a **requester role** (guest, licensee, claimant) and **action** (preview/play/download/remix) against simple local policy rules. Relevant inputs include explicit permission grants, selected licence category, and whether there is an open dispute. Results are recorded as **ALLOW** or **DENY** in the policy audit.

An ALLOW result is a **policy simulation only**: no encrypted media, licensed playback SDK, decryption keys, secure key server, streaming infrastructure, or actual copy prevention is implemented. This helps show how future DRM would consume provenance and entitlement information.

### 7. Node/indexer diagnostics

The **Node & Events** screen contains a synthetic block height, pending job queue, and event console. You can mine pending jobs, **pause** indexing so chain state advances before application indexing, **catch up** indexed jobs, and **reorganize the last simulated block** to show a rollback. This is a deliberately simplified illustration, not Bitcoin consensus or a general-purpose rollback engine.

### 8. JSON state and persistence

A single in-memory state object drives all screens. The app persists it in the browser's `localStorage` under `nexusord_mvp_v3`. The initial sample dataset lives inside a `<script id="seed-data" type="application/json">` block. There is no central server or cross-device synchronization.

At **How It Works → Portable JSON data**:

- **Download current state** exports a `nexusord-demo-state.json` snapshot.
- **Import saved state** restores a matching version-3 snapshot.
- **Run consistency checks** evaluates basic relationships such as unique IDs, 64-character hex digests, supply and balance integrity, reference consistency, and indexer height.
- **Reset** restores the original samples and clears this demo's local progress after confirmation.

*Note:* local selected-file bytes and temporary object URLs do not survive refresh or JSON export/import; exported state describes the asset and hashes, not its audio payload.

## Prototype scope and accuracy

| Capability | This file | Production requirement |
|---|---|---|
| Browser UI + connected JSON state | Implemented | Authentication, permissions, server persistence, observability |
| Local SHA-256 for supplied bytes | Real browser cryptography | Secure ingestion, durable content storage, scalable fingerprint pipeline |
| Manifest signing and verification | Browser ECDSA P-256 demonstration | Identity verification, trustworthy signing keys, revocation/rotation, legal evidence review |
| Ordinals commit/reveal and IDs | Synthetic lifecycle, plausible ID shape | Bitcoin Core, `ord`, wallet, inscription serialization, fees, confirmations, reorg handling |
| Indexer and event stream | Local state machine + logs | Bitcoin block data, chain sync, durable indexed DB and recovery |
| BRC-20 deploy/mint/transfer | Simplified state simulation | Protocol-compatible parser/indexer, wallet, consensus/index rules, safety controls |
| Copyright/licence claims | Self-declared fields, versions and review simulation | Contracts, authorship/rights validation, disputes, adjudication and provenance policy |
| AI piracy detection | Scenario presets + optional exact SHA-256 | Audio fingerprint/embedding models, monitoring, review/appeal workflow |
| DRM | Rules-based ALLOW/DENY demo | Secure packaging, entitlement server, key management, players, enforcement |
| Evidence and state exports | JSON downloads, local integrity verification | Trusted provenance, external validation, retention policies and chain-of-custody controls |

## File layout

```text
.
├── index.html        # Entire executable UI, CSS, embedded seed JSON and JavaScript
├── README.md         # This repository overview and usage guide
└── WALKTHROUGH.txt   # Presenter-style live demo script and expected outcomes
```

Only `index.html` is needed for the app to run. The other two files are explanatory documentation.

## Notes for contributors and reviewers

- Treat every Bitcoin ID, block, transaction, or BRC-20 movement as **generated demo data**, not a live network result.
- Do not infer copyrights or licence grants from ownership of a satoshi, Ordinals inscription, or BRC-20 token.
- Sample artists, licences, and reference tracks are **illustrative**, not real-world rights claims.
- The licence's exact wording and the rights scope matter more than a colourful licence category badge. A claimed right can be contested.
- User inputs and export files remain local to the browser; nevertheless, **do not enter sensitive or legally confidential material** into a public prototype without a security review. Browser local storage is not encrypted secure evidence storage.
- The demo is designed for explaining intended UX and data relationships, **not** for real anti-piracy enforcement or production security decisions.

## Background reading

Based on the NEXUSORD concept and Aniket Srivastava's explainer series:

- [BRC-20 and Ordinals — Explain Like I Am 5](https://medium.com/@aniketsrivastava81/brc-20-and-ordinals-explain-like-i-am-5-7b0ae8640246)
- [BRC-20 and Ordinals — Explain Like I Am 10](https://medium.com/@aniketsrivastava81/brc-20-and-ordinals-explain-like-i-am-10-c8946a0bcdfc)
- [BRC-20 and Ordinals — Explain Like I Am 15](https://medium.com/@aniketsrivastava81/brc-20-and-ordinals-explain-like-i-am-15-85b952d447a1)
- [Inside NEXUSORD — An Ordinals and Music Provenance Concept](https://medium.com/@aniketsrivastava81/brc-20-and-ordinals-inside-nexusord-a-ordinals-and-music-provenance-concept-52886e928d5e)

**Suggested reviewer path:** Read this README, open the deployed site, click **Run a complete demonstration**, then follow `WALKTHROUGH.txt` for the deeper presentation.
