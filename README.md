# Raydium CP-Swap (CPMM) - Denial of Service (DoS) Vulnerability

## Overview
This repository contains the Proof of Concept (PoC) for a Medium severity Griefing/DoS vulnerability discovered in the Raydium CP-Swap (CPMM) smart contract. 

## Vulnerability Details
- **Vulnerability Type:** Unhandled Panic (Rust `.unwrap()` underflow)
- **Impact:** Denial of Service (DoS), Griefing, Potential failure in CPI integrations.
- **File:** `programs/cp-swap/src/curve/constant_product.rs` (Line 40)

## Root Cause
The `swap_base_output` instruction lacks a boundary check for the `amount_out_received`. When a user requests more output tokens than the pool currently holds, the `checked_sub` operation underflows. Because the code uses `.unwrap()`, the Solana program instantly panics (`ProgramFailedToComplete`) instead of returning a graceful Anchor Error.

## Proof of Concept
The PoC is written in TypeScript and can be found in `tests/dos_poc.test.ts`. It sets up a standard pool and attempts to swap for an amount larger than the pool's vault.

### Expected Output
```text
Caught Error: Raw transaction ... failed ({"err":{"InstructionError":[0,"ProgramFailedToComplete"]}})
✅ PoC SUCCESSFUL: Program panicked due to Underflow in constant_product.rs
