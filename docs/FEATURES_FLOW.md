# Secure File Sharing App - Features & Flow

## ✨ Overview

This project enables secure file sharing using blockchain concepts and distributed storage (IPFS). It encrypts user files, stores metadata immutably, and offers traceability via a blockchain-like ledger.

---

## 👤 User Roles

* `USER`: Can register, login, upload, download files, view own ledger.

---

## 📦 Core Features (Use Cases)

### 1. User Registration & Authentication

* Register with email, username, and password.
* Login via JWT-based authentication.

### 2. Secure File Upload

* User selects file.
* File is AES-encrypted locally on server.
* File is uploaded to IPFS.
* IPFS hash and file metadata are stored in PostgreSQL.
* A blockchain-like ledger entry is created.

### 3. Secure File Download

* Authenticated user downloads a file.
* File is fetched from IPFS.
* Decrypted and served back to user.

### 4. Blockchain-inspired Ledger View

* For each uploaded file, a ledger of historical uploads is visible.
* Each block is linked via SHA-256 hash to the previous.
* Ensures integrity and immutability of file metadata.

---

## 🧽 Flow Summary (Step-by-step)

### 🔐 Registration/Login

1. User registers via `/api/auth/register`.
2. Login via `/api/auth/login` returns a JWT token.

---

### 📤 File Upload

1. Authenticated request to `/api/file/upload`
2. Server:

   * Encrypts file (AES)
   * Uploads to IPFS
   * Stores IPFS hash and file details in `files` table
   * Creates new ledger block with `prev_hash → block_hash`
   * Stores this block in `ledger` table

---

### 📥 File Download

1. User requests file via `/api/file/{id}`
2. Server:

   * Fetches IPFS content
   * Decrypts file using AES key
   * Sends file to client

---

### 🧾 Ledger Inspection

1. User calls `/api/ledger/{file}`
2. Server returns block history for the file:

   * Ordered by `created_at`
   * Contains `block_hash`, `prev_hash`, timestamp, etc.
