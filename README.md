# notary

A minimalist "transparency ledger" used to provide cryptographic proof of the existence, integrity, and authorship of digital content.

## Overview
In an era of increasingly sophisticated synthetic media and "fake news" accusations, this repository serves as a proactive defense. By anchoring content to multiple cryptographic "witnesses," it establishes a verifiable chain of custody that is difficult to forge or retroactively alter.

## The Authentication Stack
This repository employs a multi-factor authentication strategy for all stored content:

1. **Integrity (SHA-512):** A raw hash of the content proves the data has not been modified by a single bit.
2. **Identity (OpenPGP):** Hashes are signed with a GnuPG private key to verify authorship.
3. **Time (RFC 3161):** PGP signatures are anchored by a Trusted Timestamping Authority (TSA) to prove the content existed at a specific point in history.
4. **Provenance (Git):** The commit history of this repository provides a public, Merkle-tree-based audit trail.

## Repository Structure
Each authenticated item is organized as follows:
- `filename.sha512`: The raw SHA-512 hash of the content.
- `filename.hash.sig`: An armored GPG cleartext signature of the hash.
- `filename.hash.sig.tsr`: An RFC 3161 timestamp response file verifying the signature.

## Verification
To verify an entry in this notary:

1. **Verify the PGP Signature:**
   ```bash
   gpg --verify filename.hash.sig

2. **Verify the Timestamp:**
   ```bash
   openssl ts -verify -data filename.hash.sig -in filename.hash.sig.tsr -CAfile freetsa.crt
