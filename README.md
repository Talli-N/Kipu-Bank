# 🏦 KipuBank – Secure Ethereum Vault

**KipuBank** is a decentralized bank smart contract built with **Solidity**, created as part of the **Kipu Web3 Developer Final Exam**.  
It demonstrates secure and maintainable smart contract practices following real-world Ethereum standards.

---

## 🔍 Overview

KipuBank allows users to:
- Deposit their own ETH into a personal vault.
- Withdraw up to a **fixed transaction limit** (`WITHDRAW_LIMIT`).
- Operate under a **global deposit cap** (`bankCap`).
- Receive clear feedback through **custom errors** and **events**.

---

## ⚙️ Smart Contract

| Feature | Description |
|----------|-------------|
| **Immutable & constant variables** | Define global rules and limits. |
| **Storage variables** | Track balances and total deposits. |
| **Events** | `Deposit`, `Withdrawal`. |
| **Custom errors** | Improve clarity and gas efficiency. |
| **Modifier** | Validates conditions like cap and balances. |
| **Private function** | `_transferFunds()` safely sends ETH. |
| **External functions** | `deposit()`, `withdraw()`. |
| **View function** | `getBalance()`. |

---

## 🛠️ Deployment

### Deploy via Remix
1. Open [Remix IDE](https://remix.ethereum.org)
2. Paste the code from `contracts/KipuBank.sol`
3. Select environment → **Injected Provider - MetaMask**
4. Network → **Sepolia Testnet**
5. Enter `bankCap` (e.g. `10 ether`)
6. Click **Deploy**, confirm in MetaMask
7. Copy the contract address from Remix logs

---

## 🌐 Verification on Etherscan

1. Go to [https://sepolia.etherscan.io](https://sepolia.etherscan.io)
2. Paste your contract address
3. Open the **Contract** tab → **Verify and Publish**
4. Choose:
   - Compiler: `0.8.30`
   - License: MIT
   - Optimization: Enabled
5. Paste your Solidity code
6. Click **Verify and Publish**

---

## 💬 Interaction

- **Deposit ETH:** call `deposit()` with value (e.g. 0.1 ETH)
- **Withdraw ETH:** call `withdraw(amount)` (max 0.5 ETH)
- **Check balance:** call `getBalance()`
- **View events:** on Etherscan, see `Deposit` and `Withdrawal`

---

## 🔐 Security Practices

- Follows **Checks–Effects–Interactions** pattern  
- Uses **custom errors** instead of string `require`  
- Safe ETH transfers with low-level `call`  
- Clear **NatSpec documentation** throughout

---

## 🌐 Deployment Info

| Parameter | Value |
|------------|--------|
| **Network** | Sepolia Testnet |
| **Contract Address** | `0x...` ← *(add your deployed address here)* |
| **Compiler** | 0.8.30 |
| **License** | MIT |

---

## 📜 License

MIT © 2025 — Created for the Kipu Web3 Developer Exam.
