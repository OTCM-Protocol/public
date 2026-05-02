# RWA Tokens

**Institutional Infrastructure for Tokenized Real-World Assets**

[![Solana](https://img.shields.io/badge/Solana-Mainnet--Beta-9945FF?logo=solana&logoColor=white)](https://solana.com)
[![Token Standard](https://img.shields.io/badge/SPL-Token--2022-14F195)](https://spl.solana.com/token-2022)
[![SEC](https://img.shields.io/badge/SEC-Category%201%20Model%20B-0E1829)](https://www.sec.gov)
[![License](https://img.shields.io/badge/License-BSL%201.1-C9A84C)](LICENSE)
[![OTC](https://img.shields.io/badge/OTC-GROO-0E7B54)](https://www.otcmarkets.com/stock/GROO/overview)
[![Codebase](https://img.shields.io/badge/Codebase-~560%2C000%20LOC-blue)]()

---

## Table of Contents

- [What is RWA Tokens?](#what-is-rwa-tokens)
- [Three Modules](#three-modules)
- [Market Context](#market-context)
- [Architecture Overview — Nine Layers](#architecture-overview--nine-layers)
- [Cross-Layer Communication Protocol](#cross-layer-communication-protocol)
- [Layer 1: Solana Blockchain Foundation](#layer-1-solana-blockchain-foundation)
- [Layer 2: Security Enforcement — 42 Transfer Hook Controls](#layer-2-security-enforcement--42-transfer-hook-controls)
- [Layer 3: Global Unified CEDEX Liquidity Pool](#layer-3-global-unified-cedex-liquidity-pool)
- [Layer 4: Custom AMM Engine (CPMM)](#layer-4-custom-amm-engine-cpmm)
- [Layer 5: CEDEX — Compliant Exchange for Digital Securities](#layer-5-cedex--compliant-exchange-for-digital-securities)
- [Layer 6: Oracle Network](#layer-6-oracle-network)
- [Layer 7: Protocol Governance](#layer-7-protocol-governance)
- [Layer 8: Wallet Infrastructure](#layer-8-wallet-infrastructure)
- [Layer 9: Predictive AI Module](#layer-9-predictive-ai-module)
- [Module 2: Real Estate Tokens](#module-2-real-estate-tokens)
- [Module 3: CORECM Tokens](#module-3-corecm-tokens)
- [Why Not Ethereum? Why Not L2?](#why-not-ethereum-why-not-l2)
- [Why Not Raydium / Orca / Jupiter?](#why-not-raydium--orca--jupiter)
- [Security Architecture](#security-architecture)
- [Codebase Composition](#codebase-composition)
- [Repository Structure](#repository-structure)
- [Tech Stack](#tech-stack)
- [Regulatory Framework](#regulatory-framework)
- [Performance Specifications](#performance-specifications)
- [Roadmap](#roadmap)
- [Getting Started](#getting-started)
- [Documentation](#documentation)
- [References](#references)
- [License](#license)

---

## What is RWA Tokens?

RWA Tokens is a purpose-built, nine-layer Solana Mainnet-Beta infrastructure platform that tokenizes real-world assets across three asset classes — global equities, commercial real estate, and mineral and hydrocarbon project interests — under a single SEC Category 1 Model B compliance backbone.

All three modules are production-ready and available for issuer onboarding today: **Module 1: ST22 Digital Securities** (global equities), **Module 2: Real Estate Tokens** (commercial real estate), and **Module 3: CORECM Tokens** (mineral, oil, and gas project interests). Module 1 reaches general availability for live trading on CEDEX at Q3 2026; Modules 2 and 3 onboard issuers under the same nine-layer framework on the same timeline.

Each tokenized instrument represents direct beneficial ownership in an underlying asset held in qualified custody by [Empire Stock Transfer](https://www.empirestock.com), an SEC §17A-registered transfer agent. Instruments operate within the **Category 1 Model B** architecture defined by [SEC Release No. 33-11412](https://www.sec.gov) (March 17, 2026) and the January 28, 2026 Joint Staff Statement on Tokenized Securities from the Division of Corporation Finance, Division of Investment Management, and Division of Trading and Markets.

The core insight: compliance enforcement in traditional markets is expensive because it requires human intermediaries at every step. When compliance is embedded in the token transfer primitive itself — enforced mathematically at the moment of every transfer by immutable smart contract code — the marginal cost of compliance per transaction approaches zero. This cost structure makes institutional-grade securities compliance viable across asset classes that traditional finance cannot service economically.

```
Transfer initiated → Token-2022 program → Transfer Hook CPI →
  42 controls validated (<10ms) → ANY failure = atomic revert (Error 6xxx) →
  ALL pass = transfer executes + immutable audit trail written on-chain
```

Operated by [Groovy Company, Inc.](https://www.otcmarkets.com/stock/GROO/overview) (OTC: GROO, CIK 1499275, Wyoming domiciled). Headquartered in Atlanta, Georgia. Wyoming law governs all RWA Tokens documents.

---

## Three Modules

| Module | Asset Class | Status |
|---|---|---|
| **ST22 Digital Securities** | Global equities — Common Class B equity tokens for OTC microcap, NASDAQ, NYSE, AMEX, TSX, LSE, Tokyo, Hong Kong, and other exchange-listed issuers | **Production** · CEDEX trading GA Q3 2026 |
| **Real Estate Tokens** | Income-producing commercial real estate via Nevada LLC → Nevada corporation pathway | **Production** · Issuer onboarding open |
| **CORECM Tokens** | Royalty and project-equity instruments for mineral, oil, and gas operators | **Production** · Operator onboarding open |

All three modules share the same nine-layer architecture, the same Empire Stock Transfer custody framework, the same 42 Transfer Hook compliance controls, and the same CEDEX trading venue. Module-specific differences are isolated to the underlying instrument, the custody artifact, and the oracle feed driving price discovery.

**Equities scope note.** Module 1 production deployment onboards OTC microcap issuers under the Empire Stock Transfer custody arrangement at general availability in Q3 2026. Expansion to NASDAQ, NYSE, AMEX, TSX, LSE, and other listing venues is sequenced through 2026–2028 and is gated on transfer agent onboarding for each issuer's existing transfer agent of record. The protocol architecture is venue-agnostic; production rollout is sequenced by the regulatory and operational onboarding cadence per venue.

---

## Market Context

The institutional infrastructure for tokenized real-world assets converged on a viable architecture across four market events in the twelve months preceding RWA Tokens' launch:

| Date | Event | Significance |
|---|---|---|
| Jan 28, 2026 | SEC Joint Staff Statement on Tokenized Securities | First definitive guidance distinguishing Category 1 (issuer-sponsored) from Category 2 (third-party-sponsored) tokenization architecture |
| Mar 17, 2026 | SEC Release No. 33-11412 | Codifies Category 1 Model B compliance — DLT integrated into the official Master Securityholder File via the SEC §17A-registered transfer agent |
| Apr 13, 2026 | SEC Staff Statement on Covered User Interface Providers | Clarifies non-broker-dealer status for compliant interface providers operating with adequate compliance controls |
| 2026 | GENIUS Act stablecoin framework | Establishes USDC and PYUSD as compliant settlement instruments; enables atomic delivery vs. payment on Solana |

Combined U.S. addressable market across the three modules: equities (global listed and OTC), commercial real estate ($11T U.S. CRE), and global mineral and hydrocarbon reserves ($4T) — totaling **$15T+** institutional addressable market.

---

## Architecture Overview — Nine Layers

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                     RWA TOKENS — NINE-LAYER STACK                           │
├──────┬──────────────────────────┬────────────────────────────────────────────┤
│  L9  │ Predictive AI Module     │ EDGAR NLP · IDOS scoring · XGBoost ML     │
│  L8  │ Wallet Infrastructure    │ iOS/Android native · Ledger/Trezor HW     │
│  L7  │ Protocol Governance      │ Multi-sig · 48h timelock · on-chain vote  │
│  L6  │ Oracle Network           │ Empire Ed25519 · OFAC · AML · TWAP · EDGAR│
│  L5  │ CEDEX Exchange           │ cedex.market · Web2 matching + Web3 settle│
│  L4  │ Custom AMM Engine        │ CPMM (x × y = k) · u128 overflow-safe    │
│  L3  │ Global Unified Liquidity │ Protocol-owned · LP burned · permanent    │
│  L2  │ Security Enforcement     │ 42 Transfer Hook controls · atomic · CPI  │
│  L1  │ Solana Foundation        │ PoH + Tower BFT · 65K TPS · ~400ms blocks │
└──────┴──────────────────────────┴────────────────────────────────────────────┘
```

**Design philosophy:** Strict separation of concerns. Each layer handles a distinct responsibility and communicates with adjacent layers through well-defined interfaces. Securities compliance enforcement occurs at **Layer 2, the lowest programmable layer**, ensuring no higher-layer component can bypass, override, or selectively apply the 42 Transfer Hook security controls. A change to Layer 4 (AMM) cannot affect Layer 2 (Security Enforcement) — compliance is structurally isolated from trading mechanics.

No layer is repurposed from general DeFi infrastructure. Each was purpose-built for tokenized real-world assets.

---

## Cross-Layer Communication Protocol

Inter-layer communication follows strict directional protocols ensuring data integrity and separation of responsibilities:

```
                    ┌─────────────────────┐
                    │  Layer 9: AI Module  │
                    │  Layer 8: Wallet     │
                    │  Layer 5: CEDEX      │
                    └─────────┬───────────┘
                              │ ▼ Downward (Request Flow)
                    ┌─────────┴───────────┐
                    │  Layer 2: Security   │◄──── Layer 6: Oracle Feeds
                    │  Enforcement         │      (Empire, OFAC, AML, TWAP)
                    └─────────┬───────────┘
                              │ ▼ Settlement
                    ┌─────────┴───────────┐
                    │  Layer 1: Solana     │
                    └─────────┬───────────┘
                              │ ▲ Upward (Response Flow)
                              │   Confirmations propagate back through
                              │   L2 → L6 → L5/L8 with audit trail
                              │   written at each layer transition
```

- **Downward (Request Flow):** Actions at L9/L8/L5 flow downward through L2 for Transfer Hook verification, drawing oracle data from L6, before settling on L1. **No higher-layer action reaches L1 without passing through L2 compliance.**
- **Upward (Response Flow):** Settlement confirmations propagate from L1 through L2 compliance acknowledgement, L6 oracle confirmation, into user-facing interfaces at L5/L8. Full on-chain audit trail at each transition.
- **Oracle Feeds (L6 → L2):** Empire custody data, OFAC/SDN updates, AML risk scores, TWAP price feeds flow directly from L6 to L2. This is the **only external data channel** influencing compliance decisions — it does not pass through the application layer. Manipulating oracle inputs requires compromising Empire Stock Transfer's custody system directly, not RWA Tokens' software.
- **Governance Writes (L7 → L2/L3/L4):** Passed proposals write to parameter configurations via 48-hour timelock. This is the only cross-layer write path not triggered by a token transfer. Governance **cannot** write to immutable Transfer Hook logic — only adjustable parameters within hard-coded bounds.
- **Horizontal:** Components within each layer communicate via internal APIs. No horizontal communication crosses layer boundaries.

---

## Layer 1: Solana Blockchain Foundation

Selected for a single decisive criterion: **SPL Token-2022 Transfer Hook support.** No other production blockchain provides native, atomic compliance enforcement at the token transfer primitive. Performance advantages reinforce the selection but are secondary to the compliance architecture capability.

### Solana Core Innovations — Why They Matter for ST22

| Innovation | Mechanism | RWA Tokens Relevance |
|---|---|---|
| **Proof of History (PoH)** | SHA-256 hash chain timestamping transactions before consensus | Deterministic ~400ms block times matching Empire custody oracle refresh |
| **Tower BFT** | PoH-optimized Byzantine Fault Tolerant consensus | Faster validator agreement via shared verifiable time reference |
| **Turbine** | BitTorrent-style block propagation in structured tree pattern | Fast propagation as Transfer Hook transaction volume scales |
| **Gulf Stream** | Mempool-less forwarding to expected leader validator | Eliminates mempool bottleneck; reduces frontrunning surface for ST22 trades |
| **Sealevel** | Parallel smart contract execution on non-overlapping accounts | **Critical:** Multiple ST22 Transfer Hook verifications execute simultaneously — enables thousands of concurrent compliance-verified trades |
| **Pipelining** | TPU validates/processes/commits through parallel hardware stages | Eliminates sequential bottlenecks in Transfer Hook processing |
| **Cloudbreak** | Horizontally scaled accounts DB with simultaneous SSD read/write | Supports high-throughput oracle reads on every Transfer Hook invocation |
| **Archivers** | Distributed ledger storage offloading validator burden | Permanent accessibility of immutable on-chain compliance audit trail |

### SPL Token-2022 Features Used

| Feature | RWA Tokens Usage |
|---|---|
| **Transfer Hooks** | 42 security controls invoked atomically on every ST22 transfer via CPI — core compliance mechanism |
| **Metadata Extension** | On-chain token metadata: issuer identity, CUSIP, Common B authorization, regulatory classification |
| **Permanent Delegate** | Empire Stock Transfer designated as permanent delegate — custody oracle verification + emergency freeze |
| **Transfer Fee Extension** | 5% protocol fee at token program level — cannot be bypassed by any trading venue |
| **Confidential Transfers** | Available for future institutional privacy — not activated in current production |

---

## Layer 2: Security Enforcement — 42 Transfer Hook Controls

The most critical innovation. The only layer that **cannot be disabled, upgraded, or worked around** by any participant — including Groovy Company, Inc. itself. Every ST22 token transfer must pass all 42 controls. No admin override. No whitelist bypass.

### Transaction Flow

```
User initiates transfer (wallet, DEX, or programmatic call)
  → Token-2022 program receives transfer instruction
  → Token-2022 invokes Transfer Hook via CPI (Cross-Program Invocation)
  → Transfer Hook validates against all 42 security controls (<10ms)
  → ANY control fails → Transaction REJECTED (atomic rollback, Error 6xxx)
  → ALL controls pass → Transfer executes + immutable audit trail written
```

**Critical Property:** The Transfer Hook cannot be removed after mint creation. Once an ST22 token is minted with the protocol's Transfer Hook, every transfer for its entire existence is validated against all 42 controls. This is an SPL Token-2022 standard property — the hook program address is permanently stored in the mint account.

### The 42 Controls — Eight Categories

| Category | Controls | Enforcement | Error Codes |
|---|---|---|---|
| **Custody Verification** | 1–6 | 1:1 Common B attestation via Empire Ed25519 oracle every ~400ms. Zero-tolerance — any discrepancy halts all transfers. | 6001 |
| **Investor Verification** | 7–14 | KYC (individuals), KYB (entities), AML risk scoring, accreditation status — all verified through Empire Stock Transfer as sole onboarding authority | 6002–6003 |
| **Position Limits** | 15–19 | 4.99% max wallet concentration per ST22 token. Velocity controls prevent rapid accumulation. u128 overflow-safe arithmetic. | 6004 |
| **Circuit Breakers** | 20–23 | >10% price move in 5 min → 15-min halt. >2% single-trade price impact → block. Oracle failure → halt. Modeled on NYSE Rule 80B / NASDAQ halt procedures. | 6005–6006 |
| **Holding Period** | 24–29 | Rule 144 six-month lock (US accredited, Reg D). Reg S twelve-month distribution compliance (non-US). Enforced on-chain — no early unlock path. | 6024 |
| **Sanctions Compliance** | 30–34 | Three-layer OFAC screening: exact wallet match, fuzzy entity matching, 2-hop clustering analysis. Chainalysis KYT + TRM Labs dual-provider. | 6007 |
| **Protective Conversion** | 35–38 | Automatic triggers on adverse events (custody discrepancy, regulatory action, issuer eligibility lapse) | 6008 |
| **Governance / Audit** | 39–42 | Immutable compliance audit trail on every transfer. Control 42: regulatory freeze requiring Legal Counsel authorization + 3-of-5 multi-sig. | 6042 |

### Transfer Hook Implementation

```rust
use anchor_lang::prelude::*;

#[program]
pub mod rwa_tokens_transfer_hook {
    use super::*;

    pub fn transfer_hook(ctx: Context<TransferHook>, amount: u64) -> Result<()> {
        let config = &ctx.accounts.config;

        // ── SEQUENTIAL: Custody + Identity (must pass before any trade logic) ──

        // Controls 1-6: Empire Stock Transfer custody verification (Ed25519 signed)
        verify_custody_backing(&ctx.accounts.custody_oracle, amount)?;    // Error 6001
        verify_1to1_ratio(&ctx.accounts.mint_supply, &ctx.accounts.custody_oracle)?;
        verify_empire_status(&ctx.accounts.empire_status_oracle)?;
        verify_custody_signature(&ctx.accounts.custody_oracle)?;          // Ed25519
        verify_mint_authority_locked(&ctx.accounts.mint)?;
        verify_backup_custody_oracle(&ctx.accounts.backup_oracle)?;

        // Controls 7-14: Investor eligibility (Empire-verified)
        check_kyc_status(&ctx.accounts.sender, config)?;                  // Error 6002
        check_kyc_status(&ctx.accounts.receiver, config)?;
        check_kyb_entity(&ctx.accounts.receiver, config)?;
        check_accreditation(&ctx.accounts.receiver, config)?;             // Error 6003

        // Controls 8-10: OFAC sanctions screening (three-layer)
        check_ofac_sender(&ctx.accounts.sender, config)?;                 // Exact match
        check_ofac_receiver(&ctx.accounts.receiver, config)?;             // Fuzzy entity
        check_ofac_oracle_freshness(&ctx.accounts.ofac_oracle)?;          // 2-hop cluster

        // Control 11: AML risk scoring
        check_aml_risk(ctx.accounts.aml_oracle.sender_score)?;
        // Disposition: 0-30 approve | 31-70 enhanced review | 71-100 reject

        // Control 24: Holding period enforcement
        enforce_holding_period(&ctx.accounts.holding_period_account)?;    // Error 6024
        // Rule 144: 6 months (US / Reg D) | Reg S: 12 months (non-US)

        // ── PARALLEL: Market integrity controls ─────────────────────────────

        check_wallet_limit(amount, config)?;          // Controls 15-19: 4.99% max
        check_price_impact(amount, config)?;          // Control 25: 2% max vs TWAP
        check_volume_halt(amount, config)?;           // Control 27: 30% daily/wallet
        check_circuit_breaker(config)?;               // Control 28: >10% in 5 min
        validate_cei_pattern(&ctx.accounts)?;         // Controls 29-35: reentrancy
        enforce_arithmetic_safety(amount)?;           // Controls 16-19: u128 overflow

        // ── PROTECTIVE + GOVERNANCE: Controls 35-42 ─────────────────────────

        check_protective_conversion_triggers(&ctx)?;  // Controls 35-38
        write_compliance_audit_trail(&ctx)?;           // Controls 39-41
        check_regulatory_freeze(&ctx.accounts.freeze_authority)?; // Control 42

        // ── EMIT: Immutable on-chain compliance record ──────────────────────

        emit!(TransferValidated {
            mint:      ctx.accounts.mint.key(),
            from:      ctx.accounts.source.key(),
            to:        ctx.accounts.destination.key(),
            amount,
            timestamp: Clock::get()?.unix_timestamp,
            all_controls_passed: true,
        });

        Ok(())
    }
}
```

### Immutability Guarantee

Transfer Hook upgrade authority: **5-of-9 multi-sig** with geographically distributed key holders. All upgrades require **24-hour timelock** (cancellable via governance). Upgrades cannot remove the 42 controls — any upgrade weakening protections requires **66.67% governance supermajority** + account schema compatibility attestation.

---

## Layer 3: Global Unified CEDEX Liquidity Pool

The defining infrastructure innovation. Every issuer that onboards inherits an immediately functional secondary market — no per-issuer liquidity sourcing, no bonding curve bootstrapping, no dependency on external market makers.

| Property | Specification |
|---|---|
| **Pool Structure** | Single shared capital reserve — all ST22 issuers draw simultaneously |
| **Funding Sources** | (1) OTCM Security Token proceeds ($20M Reg D) routed via Solana Treasury; (2) Staking Pool allocation; (3) 0.44% permanent lock on every CEDEX trade; (4) investor secondary sale proceeds; (5) 2% staking-reward reinvestment |
| **LP Token Status** | **Burned at initialization** — no withdrawal function exists in the contract |
| **Withdrawal Possibility** | Zero. Mathematical impossibility — the function was never written. |

**Why this matters:** Traditional per-issuer pools fail at scale because each token requires dedicated capital and thin markets can never justify the depth. A shared pool means issuer #500 gets the same liquidity depth as issuer #1. The pool only grows, across every issuer and every trade, permanently. Real Estate Tokens (Module 2) and CORECM Tokens (Module 3) draw against the same Global Unified Pool when activated.

---

## Layer 4: Custom AMM Engine (CPMM)

### Why a Custom AMM Exists

Every major Solana DEX — Raydium, Orca, Jupiter, Meteora — was built before SPL Token-2022 Transfer Hooks existed. Their swap programs call the original SPL Token Program directly, **bypassing Transfer Hooks at trade execution.** This is a fundamental architectural incompatibility. A custom AMM is the only path to compliant secondary trading.

### CPMM Mathematical Foundation

Implements `x × y = k` against the Global Unified CEDEX Liquidity Pool:

```
x = USDC reserve     y = ST22 token reserve     k = constant product invariant
fee = 5% (500 bps) applied BEFORE invariant calculation — k grows with every trade

Example: Buy ST22 with 10 USDC from pool with 100 USDC / 1,000,000 tokens (k = 100M)

  10 USDC input → 5% fee → 9.5 USDC effective
  new_x = 109.5 USDC
  new_y = 100,000,000 / 109.5 = 913,242 tokens
  tokens_out = 1,000,000 - 913,242 = 86,758
  k_after = 109.5 × 913,242 ≥ 100,000,000 ✓
```

### u128 Overflow-Safe Arithmetic

Max u64 = 2⁶⁴ − 1 ≈ 18.4 × 10¹⁸. Multiplying two max u64 reserves requires 128 bits. **All intermediate calculations use u128 with checked arithmetic — no unchecked operations in the swap path:**

```rust
pub fn calculate_output_amount(
    input_amount: u64, input_reserve: u64,
    output_reserve: u64, fee_bps: u16,       // 500 = 5%
) -> Result<SwapResult> {
    let fee_multiplier = (10_000 - fee_bps as u64) as u128;
    let input_with_fee = (input_amount as u128)
        .checked_mul(fee_multiplier).ok_or(SwapError::Overflow)?;

    let numerator = input_with_fee
        .checked_mul(output_reserve as u128).ok_or(SwapError::Overflow)?;
    let denominator = (input_reserve as u128)
        .checked_mul(10_000).ok_or(SwapError::Overflow)?
        .checked_add(input_with_fee).ok_or(SwapError::Overflow)?;

    // Round DOWN — always favor the pool (prevents rounding attacks)
    let output_amount = numerator
        .checked_div(denominator).ok_or(SwapError::ZeroDivision)? as u64;

    Ok(SwapResult { output_amount, fee_collected: /* ... */ })
}
```

---

## Layer 5: CEDEX — Compliant Exchange for Digital Securities

**[cedex.market](https://cedex.market)** — the only trading venue where SPL Token-2022 Transfer Hook compliance is preserved on every trade.

### Dual-Layer Execution Model

| Layer | Component | Latency |
|---|---|---|
| **Compliance Verification** | Pre-flight: 6 primary Transfer Hook queries + oracle reads | 2,000–3,000ms |
| **Trading Execution** | CPMM on-chain swap with full 42-control enforcement | 400–600ms |

**Atomicity:** If compliance passes but execution fails (slippage, deadline, insufficient balance), the entire transaction reverts. No partial state changes. No fee collected without a trade executing.

### Circuit Breaker Architecture (NYSE Rule 80B Model)

```rust
pub const MAX_PRICE_IMPACT_BPS: u16 = 200; // 2% maximum per transaction

pub fn check_price_impact(trade: &SwapParams, twap: u64, price: u64) -> Result<()> {
    let deviation_bps = ((price as i64 - twap as i64).abs() as u64)
        .checked_mul(10_000).ok_or(CedexError::Overflow)? / twap;
    require!(deviation_bps <= MAX_PRICE_IMPACT_BPS as u64,
        CedexError::PriceImpactExceeded); // Error 6006
    Ok(())
}
```

| Trade Size (% of pool) | Price Impact | Status |
|---|---|---|
| 1% | ~0.99% | Allowed |
| 2% | ~1.96% | Allowed |
| 3% | ~2.91% | **Blocked — 6006** |
| 5% | ~4.76% | **Blocked — 6006** |

### MEV Protection

[Jito Block Engine](https://jito.wtf) private transaction submission — transactions not visible in public mempool, eliminating frontrunning and sandwich attack vectors. Transfer Hook price impact controls (2% max) + TWAP deviation checks further eliminate MEV extraction economics.

---

## Layer 6: Oracle Network

Real-time data backbone feeding every Transfer Hook compliance decision. L6 feeds L2 **directly** — oracle data drives compliance without passing through the application stack. An oracle failure is a compliance failure.

### Five Oracle Categories

| Oracle | Hook Dependency | Source | Update Freq | SLA | Fail-Safe |
|---|---|---|---|---|---|
| **Custody** | Control 1 | Empire API (Ed25519) | Every block (~400ms) | <200ms | **REJECT ALL** |
| **OFAC/SDN** | Controls 8–10 | U.S. Treasury OFAC | Hourly + emergency push | <50ms | Cached <24h; halt >48h |
| **AML** | Control 11 | Chainalysis KYT + TRM | Per-transfer (cached 6h) | <400ms | Cached <6h |
| **TWAP/Price** | Control 25 | Pyth Network | Sub-second | <100ms | Last confirmed; disable breaker >5 min |
| **EDGAR** | Control 12 + L9 | SEC EDGAR RSS + EFTS | Real-time + daily batch | <60s | Last batch; IDOS flagged stale |

### Asymmetric Fail-Safe Design

Custody is the **only** hard fail-safe (reject all transfers). All others degrade gracefully on cached data. A stale AML score creates manageable risk. A custody discrepancy threatens the foundational 1:1 backing guarantee.

```
Empire → Ed25519 attestation → every ~400ms
  ├─ supply == custodied_shares → Control 1 passes
  └─ ANY discrepancy (≥1 token) → Error 6001 → ALL transfers halt
       → Circuit breaker on affected mint
       → P0 dual notification: protocol + Empire (15-min SLA)
       → Resumes only after 2-of-3 oracle consensus
```

### TWAP Parameters

| Parameter | Default | Governance Range | Immutable? |
|---|---|---|---|
| Calculation window | 30 min | 15–60 min | No |
| Minimum observations | 60 | — | Yes |
| Max price impact | 2%/transfer | 1–5% | No |
| Outlier rejection | >3σ from median | — | Yes |

**Pre-rebrand validation record:** Zero custody discrepancy events across 3 beta issuers, $7M+ processed liquidity (under predecessor brand OTCM Protocol; dba cancelled May 1, 2026).

---

## Layer 7: Protocol Governance

On-chain proposal/voting for adjustable parameters. **42 Transfer Hook controls are immutable — governance cannot weaken investor protections.** Activation target: 2028.

| Authority | Threshold | Scope | Timelock |
|---|---|---|---|
| **Parameter adjustment** | 3-of-5 multi-sig | Fees, AMM config, circuit breaker thresholds (within bounds) | 48 hours |
| **Program upgrade** | 5-of-9 multi-sig | Hook program, CEDEX, oracle programs | 24h + 66.67% supermajority + schema attestation |
| **Regulatory freeze** | 3-of-5 + Legal Counsel | Control 42 — halt transfers on specified mint | Immediate |

**What governance cannot do:** Remove/weaken any of the 42 controls. Withdraw from Global Pool. Override Control 24 holding periods. Bypass custody oracle.

---

## Layer 8: Wallet Infrastructure

| Feature | Specification |
|---|---|
| **Platforms** | Native iOS, Native Android (React Native shared core) |
| **Hardware** | Ledger + Trezor for institutional custody |
| **KYC Gate** | Empire Stock Transfer verification required before activation |
| **Routing** | All operations route through L2 Transfer Hook — wallet compliance is UX, not a substitute for on-chain enforcement |
| **Key Management** | Ledger Enterprise (HSM-backed) for institutional; Ed25519 keypairs for retail |

Even raw Solana transactions bypassing the wallet app still trigger all 42 Transfer Hook controls.

---

## Layer 9: Predictive AI Module

Commercial intelligence engine. Proprietary AI monitors SEC EDGAR + OTC Markets data, generating daily-refreshed **IDOS (Issuer Distress and Opportunity Score)** ratings for the equities universe. Module 2 and Module 3 extensions for real estate and CORECM-specific intelligence are scheduled with their respective module launches.

| Component | Technology | Performance |
|---|---|---|
| **IDOS Scoring** | XGBoost gradient boosting | AUC-ROC ≥ 0.75 production gate |
| **EDGAR NLP** | Custom entity extraction + filing classification | <60s from RSS to score update |
| **Investor Profiling** | On-chain wallet behavioral analysis (Solana) | 6h refresh + event-driven |
| **Launch Timing (LTOE)** | Solana congestion + market sentiment model | 4h refresh + real-time on congestion |
| **Model Governance** | Drift detection, emergency rollback, A/B shadow ≥2 weeks | Continuous |

Data sources: SEC EDGAR (public), OTC Markets Group (public), Solana on-chain (public). No PII/PCI/PHI. Reg D, CAN-SPAM, GDPR Article 6(1)(f) compliant.

---

## Module 2: Real Estate Tokens

Fractional ownership of income-producing commercial real estate, tokenized on the same nine-layer architecture as Module 1.

### Module-Specific Architecture

| Component | Module 2 Specification |
|---|---|
| **Underlying instrument** | Common Class B equity in a Nevada corporation holding the property; Nevada LLC → Nevada corporation conversion required for fractionalization |
| **Custody** | Empire Stock Transfer holds Common Class B shares of the property-holding corporation (same custody framework as ST22) |
| **Pricing model** | CPMM with periodic NAV reappraisal cycles (independent appraisal triggers on-chain NAV update) |
| **Initial token pricing** | +22% premium to NAV (fractionalization premium); CPMM provides continuous two-sided liquidity around NAV |
| **Circuit breakers** | ±10% in 5 minutes triggers 15-minute halt (same Layer 2 mechanism as ST22); reappraisal cycle resets reference NAV |
| **Distributions** | Net rental income distributed on-chain in USDC or PYUSD on a fixed cadence |
| **Layer reuse** | All nine layers — L2 Transfer Hooks (with property-instrument-specific custody oracle), L3 Global Unified Pool, L4 CPMM, L5 CEDEX, L6 oracle network with appraisal feed addition, L7 governance, L8 wallet, L9 AI module RE-extension |

### Status

Module 2 is production-ready against the existing nine-layer foundation and open for issuer onboarding. Pilot deal scoped: $950K net equity in an income-producing commercial property. Property appraisal oracle is the principal Module 2-specific addition to Layer 6. Empire Stock Transfer custody framework extends to Common B equity of the property-holding Nevada corporation — no new custody arrangement is required.

---

## Module 3: CORECM Tokens

Tokenized royalty and project-equity instruments for mineral, oil, and gas operators on the same nine-layer architecture.

### Module-Specific Architecture

| Component | Module 3 Specification |
|---|---|
| **Underlying instrument** | Two structures supported: (a) royalty interest stripped from the mineral estate, or (b) project-equity Common Class B in the operating entity — selected per jurisdiction and Joint Operating Agreement (JOA) constraints |
| **Custody** | Empire Stock Transfer holds the underlying instrument (royalty assignment or Common B equity) under the same §17A-registered framework |
| **Reserve attestation** | Independent reserve engineer issues SPE-PRMS-compliant report; reserve volumes hashed and signed, anchored on-chain as the token's backing record |
| **Pricing model** | CPMM with operator-encoded distribution waterfalls (working interest splits, royalty waterfalls, lifting-cost mechanics) |
| **Distributions** | Production reports trigger on-chain stablecoin distributions to token holders; quarterly attestations refresh the on-chain reserve record |
| **JOA-aware distribution logic** | On-chain waterfall logic encodes JOA terms — working interests, payouts, lifting costs — eliminating the manual reconciliation that breaks royalty distribution at scale |
| **Layer reuse** | All nine layers — L2 with reserve-attestation custody oracle, L3 Global Unified Pool, L4 CPMM with waterfall extension, L5 CEDEX, L6 oracle network with reserve-attestation and production-report feeds, L7 governance, L8 wallet, L9 AI module CORECM-extension |

### Status

Module 3 is production-ready against the existing nine-layer foundation and open for operator onboarding. The principal Module 3-specific additions are the reserve-attestation oracle (Layer 6), the JOA-aware distribution waterfall in Layer 4, and the on-chain reserve record anchoring as a Layer 2 custody-equivalent. Operator onboarding pathways are scoped against U.S.-domiciled mineral, oil, and gas operators with international expansion sequenced thereafter.

---

## Why Not Ethereum? Why Not L2?

| Requirement | Ethereum / L2 | Solana |
|---|---|---|
| **Transfer Hook** | Not native — proxy pattern at $1–$50+/tx | Native Token-2022 — atomic, ~$0.00025/tx |
| **Throughput** | 15–30 TPS (L1) | 65K+ TPS; 400–600 for compliance-verified ST22 |
| **Block time** | ~12s (L1) | ~400ms (matches Empire oracle) |
| **Parallel exec** | Sequential EVM | Sealevel — concurrent Hook verification |
| **Finality** | ~6 min (32 conf) | ~13 sec (32 conf) |
| **Bypass risk** | ERC-3643 overlay can be bypassed via direct EVM call | Transfer Hooks are **runtime-enforced** — bypass path does not exist |

---

## Why Not Raydium / Orca / Jupiter?

| DEX | Token-2022 | Transfer Hook | ST22 Compatible |
|---|---|---|---|
| Raydium | Partial | **Disabled at swap** | No |
| Orca | Limited | **Not implemented** | No |
| Jupiter | Aggregates | **Inherits limits** | No |
| Meteora | Basic | **Bypassed** | No |
| **CEDEX** | Full | **All 42 controls** | **Yes** |

**Architectural lock-in, not contractual.** External DEX swap programs call the original Token Program, bypassing all compliance controls. The token ceases to be a compliant Digital Security at first external-DEX trade.

---

## Security Architecture

### Smart Contract Security

| Measure | Detail |
|---|---|
| **Formal Verification** | Certora Prover — 6 invariant proofs |
| **Audits** | Quantstamp, Halborn, OtterSec — all Critical/High must clear pre-mainnet |
| **Bug Bounty** | Up to $100K for critical vulns |
| **Immutable Core** | No upgrade proxy in CEDEX/AMM contracts |
| **CEI Pattern** | Checks-Effects-Interactions in all CPI paths (prevents reentrancy) |

### Six Formally Verified Invariants

1. No unauthorized minting without custody oracle confirmation
2. Circulating supply ≤ Empire-custodied Common B at all times
3. No fund extraction from Global Pool (LP burned, no withdrawal fn)
4. Transfer Hook always executes on every transfer (Token-2022 property)
5. 5% fee accuracy within 1 wei tolerance
6. Circuit breaker triggers if and only if threshold conditions met

### Economic Security

| Attack Vector | Defense |
|---|---|
| Flash loans | Circuit breakers block large coordinated trades |
| Price manipulation | TWAP: 60 obs / 30 min / 3σ outlier rejection |
| Coordinated dumps | 30% daily cap/wallet + correlation detection |
| Sandwich attacks | Jito private submission + 2% impact ceiling |
| Wash trading | Timing/amount correlation with auto-blacklist |
| Reentrancy | CEI enforcement in all CPI paths (Controls 29–35) |

---

## Codebase Composition

~560,000 lines of original source code:

| Component | Language(s) | Function |
|---|---|---|
| Solana Programs | Rust | On-chain: Transfer Hook, AMM, Liquidity Pool |
| Transfer Hooks | Rust | 42 controls, CPI integration, oracle verification |
| AMM Engine | Rust / TypeScript | CPMM u128 arithmetic, fee routing, circuit breakers |
| Oracle Layer | TypeScript / Rust | Empire, OFAC, Chainalysis, TRM, Pyth feeds |
| Web App | TypeScript / React | CEDEX trading UI, issuer dashboard |
| Mobile | React Native | iOS/Android wallet + trading |
| Backend | Node.js / TypeScript | API gateway, KYC/AML routing, OMS |
| Database | MongoDB / TypeScript | State, compliance reporting, IDOS |
| DevOps | Flux / Kubernetes / YAML | GitOps deployment, orchestration |
| AI Module | Rust / TypeScript / Python | XGBoost IDOS, EDGAR NLP, wallet profiling |

---

## Repository Structure

```
rwa-tokens/
├── programs/
│   ├── transfer-hook/              # Layer 2 — 42 controls (Rust/Anchor)
│   │   ├── src/
│   │   │   ├── lib.rs              # Entry point
│   │   │   ├── controls/
│   │   │   │   ├── custody.rs             # Controls 1-6
│   │   │   │   ├── investor.rs            # Controls 7-14
│   │   │   │   ├── position.rs            # Controls 15-19
│   │   │   │   ├── circuit_breaker.rs     # Controls 20-23
│   │   │   │   ├── holding_period.rs      # Controls 24-29
│   │   │   │   ├── sanctions.rs           # Controls 30-34
│   │   │   │   ├── protective.rs          # Controls 35-38
│   │   │   │   └── governance.rs          # Controls 39-42
│   │   │   ├── state/              # On-chain account structures
│   │   │   └── errors.rs           # Error codes 6001-6042
│   │   └── tests/
│   ├── amm/                        # Layer 4 — CPMM engine
│   ├── liquidity-pool/             # Layer 3 — Global Pool
│   └── governance/                 # Layer 7 — Multi-sig + timelock
├── cedex/                          # Layer 5
│   ├── engine/                     # Order matching + execution
│   ├── api/                        # REST/WebSocket
│   ├── settlement/                 # Solana settlement
│   └── mev-protection/             # Jito integration
├── oracle-network/                 # Layer 6
│   ├── custody/                    # Empire Ed25519 attestation
│   ├── compliance/                 # OFAC + AML
│   ├── price/                      # Pyth TWAP
│   └── edgar/                      # SEC EDGAR pipeline
├── wallet/                         # Layer 8
├── ai-module/                      # Layer 9
│   ├── idos/                       # XGBoost scoring
│   ├── edgar-nlp/                  # Filing NLP
│   ├── investor-profiling/         # Wallet behavioral
│   └── ltoe/                       # Launch timing
├── modules/                        # Module 2 (Real Estate) + Module 3 (CORECM)
│   ├── real-estate/                # Module 2 — Real Estate Tokens
│   └── corecm/                     # Module 3 — CORECM Tokens
├── sdk/                            # TypeScript SDK
├── docs/                           # Whitepaper + specs
├── audits/                         # Certora + audit reports
└── infrastructure/                 # K8s, Flux, monitoring
```

---

## Tech Stack

| Component | Technology | Why |
|---|---|---|
| **Blockchain** | Solana Mainnet-Beta | Only chain with native Transfer Hook |
| **Token** | SPL Token-2022 | Hooks, Metadata, Permanent Delegate, Transfer Fee |
| **Contracts** | Rust + [Anchor](https://anchor-lang.com) 0.30+ | Type-safe, CPI-native |
| **Keys** | [Ledger Enterprise](https://enterprise.ledger.com) | HSM-backed multi-sig |
| **AML** | [Chainalysis KYT](https://chainalysis.com) + [TRM Labs](https://trmlabs.com) | Dual-provider three-tier AML |
| **Settlement** | USDC / PYUSD | GENIUS Act-compliant stablecoin |
| **Price** | [Pyth Network](https://docs.pyth.network) | Sub-second TWAP feeds |
| **RPC** | [Helius](https://docs.helius.dev) | Enterprise cluster, 400ms sync |
| **MEV** | [Jito Block Engine](https://jito.wtf) | Private mempool submission |
| **Verification** | [Certora Prover](https://docs.certora.com) | Symbolic execution, 6 invariants |
| **Audits** | Quantstamp, Halborn, OtterSec | Pre-mainnet security clearance |
| **Validator** | [Firedancer](https://firedancer.io) | Client diversity (Jump Crypto) |

---

## Regulatory Framework

| Authority | Reference | Application |
|---|---|---|
| SEC | Release No. 33-11412 (Mar 17, 2026) | Category 1 Model B Digital Securities |
| SEC | Joint Staff Statement (Jan 28, 2026) | Category 1 Model B architecture |
| SEC | Staff Statement on Covered UI Providers (Apr 13, 2026) | Non-broker-dealer interface provider compliance |
| SEC | Section 17A, Exchange Act 1934 | Empire registration + custody |
| SEC | Regulation D (17 CFR §§230.501–506) | US accredited investor exemption |
| SEC | Regulation S (17 CFR §§230.901–905) | Non-US offshore safe harbor |
| SEC | Rule 144 (17 CFR §230.144) | 6-month holding — Control 24 |
| SEC | Rules 17Ad-2 through 17Ad-13 | Transfer agent operations |
| FinCEN | BSA (31 U.S.C. §5311 et seq.) | AML program foundation |
| FinCEN | 31 CFR Part 1010 | CIP, beneficial ownership, SAR |
| OFAC | 31 CFR §§500–598 | SDN screening — Controls 30–34 |
| Wyoming | W.S. 17-16-101 et seq. | Incorporation; governing law |
| Wyoming | W.S. 34-29-101 et seq. | Digital asset property rights |
| UCC | Article 8, §8-102(a)(8) | Transfer notification → Empire MSF |
| FATF | Virtual Asset Guidance (2021) | International AML/CFT framework |

---

## Performance Specifications

| Metric | Value | Notes |
|---|---|---|
| Transfer Hook validation | <10ms | All 42 controls |
| Hook compute budget | <5,000 CU | Per invocation |
| Pre-flight compliance | 2–3 seconds | Off-chain oracle queries |
| CPMM execution | 400–600ms | On-chain post-verification |
| Full swap CU | ~800,000 CU | Complete tx incl. all hooks |
| Finality | ~13 seconds | 32 confirmations |
| Compliance-verified TPS | 400–600 | Oracle-limited, not chain-limited |
| Order book sync | 400ms | One Solana block (Helius) |
| Network fee/trade | ~$0.01–$0.05 | Base + priority fee |
| Custody oracle | <200ms | Ed25519 attestation |
| OFAC screening | <50ms | Cached SDN lookup |
| AML scoring | <400ms | Chainalysis + TRM |
| IDOS refresh | <60s | EDGAR RSS → score update |
| **Bypass possibility** | **Zero** | **Runtime-enforced** |

---

## Roadmap

| Phase | Timeline | Milestones |
|---|---|---|
| **Current state** | May 2026 | All three modules production-ready · Real Estate Tokens issuer onboarding open · CORECM Tokens operator onboarding open · Empire Stock Transfer custody framework operational across all three asset classes · pre-rebrand on-chain validation complete |
| **CEDEX Trading GA** | Q3 2026 | Mainnet · CEDEX live for ST22 trading · OTC microcap issuers at production scale · $20M OTCM Security Token (Reg D / Reg S) raise · Empire Stock Transfer integrated investor onboarding |
| **Equities Expansion** | Q2 2027 | NASDAQ pilot listings · OTCM Protocol Inc. (Florida) FINRA funding portal evaluation · Reg CF capital formation alongside Reg D / Reg S |
| **Growth** | 2027–2028 | Multi-venue equities (NYSE / AMEX / TSX / LSE / Tokyo / HK) · 1,000+ issuers across all three modules · UK / EU / UAE jurisdictions · Wormhole NTT cross-chain · Governance activation |
| **Scale** | 2028+ | Global RWA infrastructure across all three modules · EVM compatibility · institutional adoption at scale |

---

## Getting Started

### Prerequisites

```
Rust 1.75+  ·  Solana CLI 1.18+  ·  Anchor 0.30+  ·  Node.js 20 LTS  ·  Yarn 1.22+
```

### Build & Test

```bash
git clone https://github.com/groovy-company/rwa-tokens.git
cd rwa-tokens && yarn install

anchor build                    # Build all Solana programs
anchor test                     # Full test suite (localnet)

# Fuzz testing (Transfer Hook)
cd programs/transfer-hook && cargo fuzz run transfer_hook_fuzz
```

### Deploy (Devnet)

```bash
solana config set --url devnet
anchor deploy --provider.cluster devnet --program-name transfer_hook
anchor deploy --provider.cluster devnet --program-name amm
anchor deploy --provider.cluster devnet --program-name liquidity_pool

# Initialize mint with Transfer Hook (IRREVERSIBLE — hook cannot be removed)
ts-node sdk/scripts/init-mint.ts --network devnet

# Verify hook attachment
ts-node sdk/scripts/verify-hook.ts --network devnet --mint <MINT_ADDRESS>
```

### SDK

```typescript
import { RwaTokensClient } from '@rwa-tokens/sdk';

const client = new RwaTokensClient({ network: 'devnet' });

const hook = await client.verifyTransferHook(mintAddress);
// hook.controlsActive === 42
// hook.bypassPossible === false

const custody = await client.getCustodyAttestation(mintAddress);
// custody.ratio === 1.0 (exact 1:1 required)
// custody.ed25519Verified === true
```

---

## Documentation

| Document | Description |
|---|---|
| [Whitepaper V8.0](https://rwatokens.net/whitepaper) | Full technical whitepaper — 15 sections + glossary |
| [Lightpaper V8.0](https://rwatokens.net/lightpaper) | Executive summary — 10 pages |
| [Transfer Hook Spec](docs/RWA-TH-SPEC-001.md) | RWA-TH-SPEC-001 — SEC submission enclosure |
| [SEC Roadmap](docs/SEC-ROADMAP.md) | Category 1 Model B architecture + regulatory compliance |
| [ERC-3643 vs ST22](docs/ERC3643-VS-ST22.md) | T-REX (Ethereum) vs. CEDEX (Solana) analysis |
| [API Reference](docs/API.md) | CEDEX REST/WebSocket + SDK |
| [AML Policy](docs/AML-POLICY.md) | BSA/AML compliance program |

---

## References

### Securities Regulation
- SEC Release No. 33-11412 (March 17, 2026) — Category 1 Model B Digital Securities
- SEC Joint Staff Statement on Tokenized Securities (January 28, 2026)
- SEC Staff Statement on Covered User Interface Providers (April 13, 2026)
- 17 CFR §§230.501–506 (Reg D) · §§230.901–905 (Reg S) · §230.144 (Rule 144)
- 17 CFR §§240.17Ad-2–17Ad-13 (Transfer Agent Rules)

### AML / Sanctions
- 31 U.S.C. §5311 et seq. (BSA) · 31 CFR Part 1010 · 31 CFR §§500–598 (OFAC)
- FATF Virtual Asset Guidance (2021)

### Blockchain
- [Solana Architecture](https://docs.solana.com) · [SPL Token-2022](https://spl.solana.com/token-2022) · [Transfer Hook Extension](https://spl.solana.com/token-2022/extensions#transfer-hook)
- [Anchor Framework](https://anchor-lang.com) · [Wormhole NTT](https://docs.wormhole.com/wormhole/native-token-transfers) · [ERC-3643](https://docs.erc3643.org) · [Firedancer](https://firedancer.io)

### Infrastructure
- [Chainalysis KYT](https://chainalysis.com) · [TRM Labs](https://trmlabs.com) · [Pyth Network](https://docs.pyth.network)
- [Jito Block Engine](https://jito.wtf) · [Helius RPC](https://docs.helius.dev) · [Certora Prover](https://docs.certora.com)

### State Law
- Wyoming Business Corporation Act (W.S. 17-16-101 et seq.)
- Wyoming Digital Asset Statute (W.S. 34-29-101 et seq.)
- UCC Article 8, §8-102(a)(8)

---

## Security

| Channel | Contact |
|---|---|
| Responsible Disclosure | security@rwatokens.net |
| Bug Bounty | Up to $100K — [rwatokens.net/security](https://rwatokens.net/security) |
| Audit Reports | `/audits` post-mainnet |

---

## License

Business Source License 1.1 — See [LICENSE](LICENSE).

Core Transfer Hook security controls and compliance logic are source-available for audit and verification. Commercial deployment requires licensing agreement.

---

<p align="center">
  <strong>RWA Tokens</strong> — Institutional infrastructure for tokenized real-world assets.<br/>
  Operated by Groovy Company, Inc. (OTC: GROO) · Wyoming domiciled · Atlanta, Georgia<br/>
  <a href="https://rwatokens.net">rwatokens.net</a> · <a href="https://cedex.market">cedex.market</a>
</p>
