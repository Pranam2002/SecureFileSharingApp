# Secure File Sharing App - Blockchain-inspired Ledger Structure

## 🔄 Overview

The application implements a lightweight, append-only, blockchain-inspired ledger to track file metadata in an immutable way. Each record (block) contains a cryptographic hash linking it to the previous block.

---

## 🔎 Ledger Block Structure

Each block is stored in the `ledger` table in the PostgreSQL database. It contains a cryptographic hash of:

* the metadata (file ID, IPFS hash, uploader ID, timestamp)
* the hash of the previous block

### Sample JSON (for conceptual reference only):

```json
{
  "file_id": "1a2b3c4d-5678...",
  "ipfs_hash": "QmXyz...",
  "uploader_id": "u123-...",
  "timestamp": "2025-06-20T12:00:00Z",
  "prev_hash": "abc123...",
  "block_hash": "sha256(prev_hash + file_id + ipfs_hash + uploader_id + timestamp)"
}
```

---

## 🔗 Hashing Logic

* Uses `SHA-256` algorithm (Java: `MessageDigest.getInstance("SHA-256")`)
* Block hash is generated by concatenating key fields as a string and hashing it

### Concatenation Format (pseudocode):

```
input = prev_hash + file_id + ipfs_hash + uploader_id + timestamp
block_hash = SHA256(input)
```

---

## 📉 Chain Construction

* Each block is linked to the previous block via `prev_hash`
* Genesis block has `prev_hash = null`
* This creates a chain where tampering with one block breaks the chain

---

## 📅 Ledger Ordering

* Ledger entries are ordered by `created_at` timestamp
* Can be queried to show file upload history for auditing purposes

---

## ⚡ Benefits

* Guarantees integrity of file uploads
* Easy to audit historical changes
* Tamper-evident structure without running a full blockchain
