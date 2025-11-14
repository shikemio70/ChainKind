
# ChainKind - DePo Aggregator + Donation Protocol

*A Decentralized Price Oracle & Philanthropy System on Stacks*

## Overview

This project implements two major on-chain systems in a single Clarity smart contract:

### **1. DePo – Decentralized Price Oracle Aggregator**

A secure, multi-provider oracle that aggregates asset prices from authorized providers, validates them, and outputs a reliable median-like price. The contract enforces strict provider permissions, price validation rules, and protection against stale or manipulated data.

### **2. On-chain Donation & Certificate System**

A lightweight crowdfunding-style donation module that allows users to create causes, donate STX, and receive non-fungible *donation certificates*. When a cause reaches its target, its recipient can withdraw the raised funds.

Both systems combine to form a fully on-chain, trust-minimized infrastructure for transparent markets and transparent giving.

---

# ✨ Features

## 🔮 Price Oracle Aggregator

* **Multi-provider price submissions**
* **Owner-managed provider registry**
* **Validation rules**:

  * Min/max bounds
  * Minimum provider count
  * Price age limits
  * Basic deviation filtering
* **Automatic price aggregation** (current implementation uses minimum price as placeholder for median)
* **Historical price storage** (structure included)
* **Error-mapped error codes** for human-readable debugging
* **Provider tracking** & last-update block recording

---

## 💝 Donation System

* **Create charitable causes** with:

  * Name
  * Target amount
  * Recipient address

* **Donate STX** to any cause

* **Automatic NFT “donation certificate” minting**

* **Track donations** per donor & cause

* **Release funds** to recipient once target is reached

* **Cause deletion after payout**

---

# 📂 Contract Structure

### **Oracle Components**

| Item                   | Description                            |
| ---------------------- | -------------------------------------- |
| `price-providers`      | Registry of authorized price reporters |
| `provider-prices`      | Last submitted price per provider      |
| `provider-last-update` | Block timestamp for each submission    |
| `active-provider-list` | Indexed provider list                  |
| `current-price`        | Aggregated price output                |
| `last-update-block`    | Last valid price timestamp             |

### **Donation Components**

| Item                   | Description                  |
| ---------------------- | ---------------------------- |
| `causes`               | Registry of donation causes  |
| `donations`            | Per-donor donation records   |
| `donation-certificate` | NFT minted for each donation |
| `next-cause-id`        | Auto-incrementing cause ID   |
| `next-certificate-id`  | NFT ID counter               |

---

# 🔑 Key Public Functions

### Oracle

| Function                   | Description                      |
| -------------------------- | -------------------------------- |
| `add-price-provider`       | Owner adds oracle provider       |
| `remove-price-provider`    | Owner removes provider           |
| `submit-price`             | Providers submit validated price |
| `get-current-price`        | Read active aggregated price     |
| `get-price-provider-count` | Number of active providers       |

### Donations

| Function         | Description                           |
| ---------------- | ------------------------------------- |
| `create-cause`   | Creates a new donation cause          |
| `donate`         | Donates STX and mints certificate     |
| `disburse-funds` | Sends raised funds to cause recipient |
| `get-cause`      | View cause data                       |
| `get-donation`   | View a donor’s donation               |

---

# 🛠 Requirements

* **Stacks network** (Mainnet/Testnet/Devnet)
* **Clarity 2.x compatible environment**
* Deployer must assign contract ownership (defaults to `tx-sender` at deploy)

---

# 🚀 How to Use

### 1. **Deploy the Contract**

Deploy via Clarinet or Hiro Wallet deployment.

### 2. **Add Price Providers**

Only owner can run:

```clarity
(add-price-provider 'SPXXXX...)
```

### 3. **Submit Prices**

Authorized provider submits:

```clarity
(submit-price u12345678)
```

### 4. **Create a Cause**

```clarity
(create-cause "Save the Ocean" u10000000 'SPRecipient...)
```

### 5. **Donate**

```clarity
(donate u1 u500000)
```

### 6. **Disburse Funds**

Recipient withdraws when target reached:

```clarity
(disburse-funds u1)
```

---

# 🧪 Testing Suggestions

* Validate price rejection (too low / too high / stale)
* Add/remove providers and ensure indexes stay correct
* Ensure NFT minting works per donation
* Verify cause deletion after disbursal
* Attempt unauthorized disbursal (should fail)

---

# 🔒 Security Considerations

* Do not exceed provider list length (`MAX_PRICE_PROVIDERS`)
* Validate all `tx-sender` permissions
* Always check STX transfer results via `try!`
* Avoid using prices if fewer than `MIN_PRICE_PROVIDERS`

---
