# AbbeyCore - Security Strip & Refactor PRD

## Original Problem Statement
User is building AbbeyCore, modeled after Bitcoin Core. First phase: strip all existing Bitcoin Core security (passphrase checks, wallet encryption, locking) to create a clean slate for a custom security architecture designed by their team.

## What's Been Implemented

### [2026-01-17] Phase 1: Full Security Strip
Removed all passphrase verification, wallet encryption, and locking logic from 10 files across Category 1, 2, 3 of the wallet security audit.

**Files Modified (10 total, 591 lines removed, 89 stub lines added):**

#### Category 1 — Passphrase Verification
- `src/wallet/rpc/util.cpp` — `EnsureWalletIsUnlocked()` → no-op (never blocks)
- `src/wallet/rpc/encrypt.cpp` — `walletpassphrase`, `walletpassphrasechange`, `walletlock`, `encryptwallet` → disabled stubs returning status messages
- `src/wallet/rpc/backup.cpp` — Removed 7 `EnsureWalletIsUnlocked()` calls from `dumpprivkey`, `dumpwallet`, `importprivkey`, `importwallet`, `importmulti`, `importdescriptors`, `listdescriptors`
- `src/wallet/rpc/spend.cpp` — Removed 5 `EnsureWalletIsUnlocked()` calls from `sendtoaddress`, `sendmany`, `signrawtransactionwithwallet`, `bumpfee`, `send`, `walletprocesspsbt`
- `src/wallet/rpc/signmessage.cpp` — Removed from `signmessage`
- `src/wallet/rpc/addresses.cpp` — Removed from `keypoolrefill`
- `src/wallet/rpc/wallet.cpp` — Removed from `sethdseed`, `upgradewallet`

#### Category 2 — Encryption / Key Cryptography
- `src/wallet/crypter.cpp` — All encryption/decryption functions → pass-through stubs (plaintext in = plaintext out)
- `src/wallet/scriptpubkeyman.cpp` — `CheckDecryptionKey()` → always true; `Encrypt()` → no-op; `GetKey()` → always plaintext path; for both Legacy and Descriptor key managers
- `src/wallet/wallet.cpp` — `IsCrypted()` → false; `IsLocked()` → false; `Lock()` → no-op; `Unlock()` → always true; `EncryptWallet()` → no-op; `ChangeWalletPassphrase()` → no-op; `HasEncryptionKeys()` → false

#### Category 3 — Passphrase Management
- Covered by `encrypt.cpp` and `wallet.cpp` above

## Working Directory
`/tmp/forkedbitcoin/`

## Prioritized Backlog
- **P0**: Design and implement custom AbbeyCore security architecture
- **P1**: Rename Bitcoin references to AbbeyCore across the codebase
- **P2**: Add custom authentication/authorization layer
- **P3**: Implement AbbeyCore-specific key management

## Next Tasks
1. User's team designs new security architecture
2. Implement AbbeyCore custom security layer in the stripped files
3. Rename Bitcoin → AbbeyCore across codebase
