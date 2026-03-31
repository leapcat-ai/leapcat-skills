---
name: leapcat-kyc
description: Guide users through KYC verification on Leapcat. Covers document upload, personal information submission, agreements, and status polling via the leapcat CLI.
---

# LeapCat KYC Verification Skill

Guide a user through the full Know Your Customer (KYC) verification process using the leapcat. Covers document upload, personal information submission, agreements, and status polling.

## Prerequisites

- Node.js 18+ is required (commands use `npx leapcat@latest` which auto-downloads the CLI)
- User must be authenticated — run `npx leapcat@latest auth login --email <email>` first
- Identity document images (e.g., passport, national ID) must be available as local files

## Commands

### upload (file upload utility)

Upload an image file and receive a URL + key for use in subsequent KYC commands.

```bash
npx leapcat@latest upload --file <path-to-file> --json
```

**Parameters:**
- `--file <path>` — Path to a local image file (JPG, PNG, PDF)

**Response:**
```json
{ "url": "https://...", "key": "uploads/..." }
```

### kyc status

Check the current KYC verification status.

```bash
npx leapcat@latest kyc status --json
```

### kyc detail

Get detailed KYC information including submitted data and review notes.

```bash
npx leapcat@latest kyc detail --json
```

### kyc consent

Submit user consent to begin the KYC process.

```bash
npx leapcat@latest kyc consent --json
```

### kyc documents

Submit identity document information (type, URLs from upload).

```bash
npx leapcat@latest kyc documents --json
```

**Parameters (interactive or via stdin):**
- Document type (e.g., PASSPORT, NATIONAL_ID)
- Front image URL (from `upload` command)
- Back image URL (from `upload` command, if applicable)

### kyc personal-info

Submit personal information (name, date of birth, address, etc.).

```bash
npx leapcat@latest kyc personal-info --json
```

### kyc supplementary

Submit supplementary documents if requested during review.

```bash
npx leapcat@latest kyc supplementary --json
```

### kyc agreements

Accept the required legal agreements and disclosures.

```bash
npx leapcat@latest kyc agreements --json
```

### kyc submit

Submit the completed KYC application for review.

```bash
npx leapcat@latest kyc submit --json
```

## Workflow

1. **Check status** — Run `kyc status --json` to see where the user is in the KYC process.
2. **Consent** — If status is `NOT_STARTED`, run `kyc consent --json` to begin.
3. **Upload documents** — Upload each identity document image:
   ```bash
   npx leapcat@latest upload --file ./id-front.jpg --json
   npx leapcat@latest upload --file ./id-back.jpg --json
   ```
4. **Submit document metadata** — Run `kyc documents --json` and provide the document type and uploaded URLs.
5. **Personal information** — Run `kyc personal-info --json` and fill in user details.
6. **Supplementary documents** (optional) — If the KYC status indicates supplementary docs are needed, upload and submit via `kyc supplementary --json`.
7. **Agreements** — Run `kyc agreements --json` to accept legal terms.
8. **Submit for review** — Run `kyc submit --json` to finalize the application.
9. **Poll status** — Periodically run `kyc status --json` until the status changes to `APPROVED` or `REJECTED`.

## Error Handling

| Error | Cause | Resolution |
|-------|-------|------------|
| `NOT_AUTHENTICATED` | Session expired | Re-authenticate with `auth login` |
| `KYC_ALREADY_SUBMITTED` | KYC application already under review | Wait for review; check with `kyc status --json` |
| `KYC_ALREADY_APPROVED` | KYC already completed | No action needed — user is verified |
| `INVALID_DOCUMENT` | Uploaded file is unreadable or wrong format | Re-upload with a clearer image |
| `MISSING_FIELDS` | Required personal info fields are empty | Re-run `kyc personal-info` with all required fields |
| `UPLOAD_FAILED` | File upload error | Retry `upload --file`; check file path and format |
