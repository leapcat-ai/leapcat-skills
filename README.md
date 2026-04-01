# Leapcat Skills

AI agent skills for [Leapcat](https://leapcat.ai) — stock trading, IPO subscription,
wallet management, KYC verification, and market data.

## Install

### Prerequisites

- Node.js 18+ (commands use `npx leapcat@0.1.0` which auto-downloads the CLI, no global install needed)

### Add skills to your project

```bash
npx skills add leapcat-ai/leapcat-skills
```

Install a specific skill:

```bash
npx skills add leapcat-ai/leapcat-skills --skill leapcat-market
```

Install globally:

```bash
npx skills add leapcat-ai/leapcat-skills -g
```

## Available Skills

| Skill | Description |
|-------|-------------|
| **leapcat-auth** | Login, logout, session management, trade password |
| **leapcat-kyc** | KYC identity verification with document upload |
| **leapcat-ipo** | Browse, estimate, subscribe to IPO projects |
| **leapcat-trading** | Place, monitor, and cancel stock orders |
| **leapcat-wallet** | Balance, deposit, withdraw, debt status |
| **leapcat-portfolio** | Portfolio overview and stock positions |
| **leapcat-market** | Real-time quotes, K-line, indices, stock search |

## Quick Start

After installing skills, authenticate with your Leapcat account:

```bash
npx leapcat@0.1.0 auth login --email your@email.com --send-only --json
npx leapcat@0.1.0 auth login --email your@email.com --otp-id <id> --otp-code <code> --json
```

Then try fetching a stock quote:

```bash
npx leapcat@0.1.0 market quote --symbol 00700.HK --json
```

## Supported Agents

Works with any AI agent that supports the open skills ecosystem:
Cursor, Claude Code, Codex, OpenCode, and 40+ others.

## License

MIT
