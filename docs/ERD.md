# Secure File Sharing App - Entity Relationship Design (ERD)

## 📊 Overview

This document defines the relational schema for the PostgreSQL database used in the Secure File Sharing App.

---

## 📄 Table: users

| Field          | Type      | Constraints                |
| -------------- | --------- | -------------------------- |
| id             | UUID      | PRIMARY KEY                |
| username       | VARCHAR   | UNIQUE, NOT NULL           |
| password\_hash | TEXT      | NOT NULL                   |
| email          | VARCHAR   | UNIQUE, NOT NULL           |
| created\_at    | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP |

---

## 📄 Table: files

| Field           | Type      | Constraints                |
| --------------- | --------- | -------------------------- |
| id              | UUID      | PRIMARY KEY                |
| user\_id        | UUID      | FOREIGN KEY → users(id)    |
| filename        | TEXT      | NOT NULL                   |
| ipfs\_hash      | TEXT      | NOT NULL                   |
| encryption\_key | TEXT      | AES key (encrypted)        |
| created\_at     | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP |

---

## 📄 Table: ledger

| Field       | Type      | Constraints                |
| ----------- | --------- | -------------------------- |
| id          | UUID      | PRIMARY KEY                |
| file\_id    | UUID      | FOREIGN KEY → files(id)    |
| block\_hash | TEXT      | SHA-256 of metadata+prev   |
| prev\_hash  | TEXT      | Nullable for genesis block |
| created\_at | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP |

---

## 📄 Table: file\_access *(optional)*

| Field        | Type      | Constraints                |
| ------------ | --------- | -------------------------- |
| id           | UUID      | PRIMARY KEY                |
| file\_id     | UUID      | FOREIGN KEY → files(id)    |
| accessed\_by | UUID      | FOREIGN KEY → users(id)    |
| timestamp    | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP |

---

## 🔎 Notes

* All `UUID` values are generated server-side using secure random UUIDs.
* Timestamps are set automatically.
* Indexes will be added to `file_id`, `user_id` for performance.
* Passwords are stored as bcrypt hashes.
* AES encryption keys are stored encrypted using a server-side master key (handled in logic).
