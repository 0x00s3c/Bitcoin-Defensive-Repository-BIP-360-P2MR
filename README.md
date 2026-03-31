---

# 🛡️ Bitcoin-Defensive-Repository-BIP-360-P2MR

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Protocol: BIP-360](https://img.shields.io/badge/Protocol-BIP--360-blue)](https://github.com/bitcoin/bips)
[![Stage: Alpha-Testnet](https://img.shields.io/badge/Stage-Alpha--Testnet-orange)](#)

## 📌 Project Overview
**Aegis-PQ: Bitcoin Edition** is a defensive implementation designed to protect the Bitcoin network from "Quantum Cracking" via **Shor’s Algorithm**. 

As we approach the era of Cryptographically Relevant Quantum Computers (CRQCs), standard ECDSA signatures become vulnerable. This repository provides a hardened fork of the **Bitcoin Quantum Testnet (v0.3.0+)**, implementing the **Pay-to-Merkle-Root (P2MR)** protocol to hide public keys and integrate Post-Quantum Cryptography (PQC).

### 🗝️ The Core Defense: BIP-360
Unlike traditional Bitcoin addresses, **BIP-360** utilizes a "Hidden Key" strategy. 
* **Key Hiding:** Public keys are never revealed on-chain until the moment of spending.
* **Hybrid Security:** Transactions require both a classical ECDSA signature and a **NIST-standardized ML-DSA (Dilithium)** signature.
* **Quantum Resistance:** Even if a quantum attacker breaks the ECDSA layer, the lattice-based PQC layer remains computationally infeasible to crack.

---

## 🚀 Getting Started

### Prerequisites
* **OS:** Linux (Ubuntu 24.04+ recommended)
* **Dependencies:** `liboqs` (Open Quantum Safe), `libdb-dev`, `boost-dev`
* **Compiler:** GCC 13+ or Clang 17+

### Installation & Build
```bash
# 1. Clone the repository
git clone https://github.com/0x00s3c/Bitcoin-Defensive-Repository-BIP-360-P2MR.git
cd Bitcoin-Defensive-Repository-BIP-360-P2MR-

# 2. Build the Bitcoin-Quantum core
./autogen.sh
./configure --with-pqc-provider=liboqs
make -j$(nproc)

# 3. Launch the Quantum Testnet Node
./src/bitcoind -testnet4 -debug=pqc
```

---

## 🛠️ Technical Specification

### 1. New Opcode Integration
We have introduced the proposed PQC opcodes into the Bitcoin Script interpreter:
* `OP_CHECKSIG_MLDSA`: Validates a Dilithium-based signature.
* `OP_P2MR_VERIFY`: Ensures the Merkle proof matches the hidden quantum root.

### 2. Transaction Structure (SegWit v3)
P2MR data is stored in the **Witness** section to maintain backward compatibility. The witness stack for a defensive spend looks like:
1. `<ML-DSA_Signature>`
2. `<ECDSA_Signature>`
3. `<Public_Key_Reveal>`
4. `<Merkle_Proof>`

---

## 📊 Threat Model Comparison

| Feature | Standard Bitcoin (P2TR) | Aegis-PQ (BIP-360) |
| :--- | :--- | :--- |
| **Primary Algorithm** | Schnorr (ECC) | **Hybrid ECC + ML-DSA** |
| **Quantum Status** | Vulnerable | **Resistant** |
| **Key Exposure** | On first spend | **Only within Merkle Root** |
| **Signature Size** | ~64 Bytes | **~2.5 KB (Witness Data)** |

---

## 🤝 Contributing
We welcome security researchers and cryptographers. If you find a vulnerability in our lattice-based implementation, please open an issue labeled `quantum-vulnerability`.

## 📜 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Would you like me to create the corresponding `README.md` for your Solana repository as well?**
