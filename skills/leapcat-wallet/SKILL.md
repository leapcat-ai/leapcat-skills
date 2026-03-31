# LeapCat Wallet Management Skill

Manage wallet balances, deposits, withdrawals, debt status, and fund activity using the leapcat.

## Prerequisites

- `leapcat` must be installed and in PATH
- User must be authenticated — run `leapcat auth login --email <email>` first
- For withdrawals: a Turnkey session is required — run `leapcat auth reauth --json` first
- For withdrawals: trade password must be set

## Commands

### wallet balance

Check the user's current wallet balance.

```bash
leapcat wallet balance --json
```

### wallet deposit-address

Get the deposit address for receiving funds.

```bash
leapcat wallet deposit-address --json
```

### wallet deposits

List all deposit transactions.

```bash
leapcat wallet deposits --json
```

### wallet deposit

Get details of a specific deposit.

```bash
leapcat wallet deposit --id <deposit-id> --json
```

**Parameters:**
- `--id <deposit-id>` — The deposit transaction identifier

### wallet withdraw

Initiate a withdrawal. Requires an elevated session (`auth reauth`) and trade password.

```bash
leapcat wallet withdraw --amount <amount> --address <address> --json
```

**Parameters:**
- `--amount <amount>` — Amount to withdraw
- `--address <address>` — Destination wallet address

### wallet withdrawals

List all withdrawal transactions.

```bash
leapcat wallet withdrawals --json
```

### wallet debt-status

Check if the user has any outstanding debt or margin obligations.

```bash
leapcat wallet debt-status --json
```

### wallet fund-activities

View a history of all fund activities (deposits, withdrawals, trades, fees, etc.).

```bash
leapcat wallet fund-activities --json
```

## Workflow

### Checking Balance

```bash
leapcat wallet balance --json
```

### Depositing Funds

1. **Get deposit address** — Run `wallet deposit-address --json` to obtain the address.
2. **Share address with user** — Provide the address so the user can transfer funds from an external source.
3. **Monitor deposit** — Periodically run `wallet deposits --json` to check if the deposit has arrived and been confirmed.

### Withdrawing Funds

1. **Re-authenticate** — Run `leapcat auth reauth --json` to elevate the session (Turnkey session).
2. **Initiate withdrawal** — Run `wallet withdraw --amount <amount> --address <address> --json`. The user will be prompted for their trade password.
3. **Monitor withdrawal** — Run `wallet withdrawals --json` to track the withdrawal status.

### Reviewing Activity

```bash
leapcat wallet fund-activities --json
```

## Error Handling

| Error | Cause | Resolution |
|-------|-------|------------|
| `NOT_AUTHENTICATED` | Session expired | Re-authenticate with `auth login` |
| `REAUTH_REQUIRED` | Elevated session needed for withdrawal | Run `auth reauth --json` first |
| `TRADE_PASSWORD_NOT_SET` | Trade password required for withdrawal | Set via `auth trade-password set` |
| `INSUFFICIENT_BALANCE` | Not enough funds to withdraw | Check balance and adjust amount |
| `INVALID_ADDRESS` | Destination address is malformed | Verify the withdrawal address |
| `WITHDRAWAL_LIMIT_EXCEEDED` | Amount exceeds daily/monthly limit | Reduce the withdrawal amount |
| `DEPOSIT_NOT_FOUND` | Invalid deposit ID | Re-check with `wallet deposits --json` |
