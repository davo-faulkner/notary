# notary

A minimalist "transparency ledger" used to provide cryptographic proof of the existence, integrity, and authorship of digital content.

## Overview
In an era of increasingly sophisticated synthetic media and "fake news" accusations, this repository serves as a proactive defense. By anchoring content to multiple cryptographic "witnesses," it establishes a verifiable chain of custody that is difficult to forge or retroactively alter.

## The Authentication Stack
This repository employs a multi-factor authentication strategy for all stored content:

1. **Integrity (SHA-512):** A raw hash of the content proves the data has not been modified by a single bit.
2. **Identity (OpenPGP):** Hashes are signed with a GnuPG private key to verify authorship.
3. **Time (Fast - RFC 3161):** PGP signatures are anchored by a Trusted Timestamping Authority (TSA) to prove the content existed at a specific point in history.
4. **Time (Immutable - OpenTimestamps):** Signatures are anchored to the Bitcoin blockchain via OTS, providing a decentralized and permanent proof of existence.
5. **Provenance (Git):** The commit history of this repository provides a public, Merkle-tree-based audit trail.
6. **Permanence (Wayback Machine):** A third-party historical snapshot of this repository. This serves as a public witness to the ledger's state, preventing retrospective alteration and ensuring public permanence.

## Repository Structure
Each authenticated item is organized as follows:
- `filename.sha512`: The raw SHA-512 hash of the content.
- `filename.sha512.sig`: An armored GPG cleartext signature of the hash.
- `filename.sha512.sig.tsr`: An RFC 3161 timestamp response file verifying the signature.
- `filename.sha512.sig.ots`: An OpenTimestamps proof file anchored to the Bitcoin blockchain.

## Redundancy & Public Permanence
To ensure this ledger remains accessible even if the primary host is unavailable, the following external witnesses are utilized:

1. **Internet Archive (Wayback Machine):** A snapshot of this repository is captured at [archive.org](https://web.archive.org/save/https://github.com/davo-faulkner/notary) following every major push to the `main` branch.
2. **OpenTimestamps:** Every notarized entry is anchored to the Bitcoin blockchain to provide decentralized proof of existence.

## Verification
To verify an entry in this notary:

1. **Verify the PGP Signature:**
   ```bash
   gpg --verify filename.sha512.sig

2. **Verify the Timestamp:**
   ```bash
   openssl ts -reply -in filename.sha512.sig.tsr -text

3. **Verify the Bitcoin Blockchain Anchor:**

   Upload `filename.sha512.sig` and `filename.sha512.sig.ots` to [OpenTimestamps.org](https://opentimestamps.org/#info) to verify the decentralized proof of existence.

## Automation
Submissions to this ledger are managed via the [`no`](https://github.com/davo-faulkner/no) script, which codifies the 6-layer authentication stack for consistent execution.
