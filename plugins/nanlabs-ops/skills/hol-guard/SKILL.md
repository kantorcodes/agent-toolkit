---
name: hol-guard
description: >-
  HOW - Set up, verify, and troubleshoot HOL Guard local protection for supported
  AI coding harnesses. Use when a user wants to inspect Guard status, detect a
  harness, run guided or manual setup, perform a dry run, launch through Guard,
  or review approvals and receipts.
metadata:
  author: nanlabs
  version: "1.0"
---

# HOL Guard local runtime safety

Use HOL Guard as an additional local protection layer around supported AI coding
harnesses. Native client sandboxing, permission controls, and organization policy
remain authoritative. Do not weaken them to make Guard work.

## When to use

- Check whether HOL Guard is running and what it is protecting.
- Detect a supported local harness before installation.
- Run guided or manual Guard setup.
- Test a harness through Guard before a protected launch.
- Review queued approvals, diffs, receipts, or troubleshooting evidence.

## Safety rules

- Never print, copy, or persist credentials, tokens, or private configuration in
  chat, logs, issues, or commits.
- Never edit Guard's local database or trust material directly.
- Never auto-approve a queued request. Approval is a trust decision and must stay
  explicit.
- Prefer the narrowest harness and approval scope that solves the task.
- Do not use `hol-guard init --yes` unless unattended setup was explicitly
  requested and side effects are acceptable.
- If `hol-guard` is not installed, stop and use the official HOL Guard
  installation documentation. Do not invent a package-manager command.

## Procedure

1. Inspect the current state before changing anything:

   ```bash
   hol-guard status --json
   hol-guard detect --json
   ```

2. For guided first-run setup, use the interactive flow:

   ```bash
   hol-guard init
   ```

   Use the manual discovery path when each setup step should be inspected:

   ```bash
   hol-guard bootstrap
   ```

3. Use the exact harness identifier requested by the user or returned by
   detection. Common identifiers include `codex`, `claude`, `cursor`, `gemini`,
   and `opencode`. Install Guard for that harness only when installation is in
   scope:

   ```bash
   hol-guard install <harness>
   ```

4. Run a dry pass before a live protected launch:

   ```bash
   hol-guard run <harness> --dry-run
   ```

5. If detection, installation, or the dry run is inconsistent, diagnose before
   changing more state:

   ```bash
   hol-guard doctor <harness> --json
   hol-guard diff <harness>
   ```

6. Launch through Guard after the dry run is understood:

   ```bash
   hol-guard run <harness>
   ```

7. Review decisions and evidence without bypassing the approval flow:

   ```bash
   hol-guard approvals
   hol-guard receipts
   hol-guard status --json
   ```

   Only run `hol-guard approvals approve <request-id>` or a denial command after
   the user has made that decision.

## Output contract

Report only what is needed to continue safely:

- detected harness or harnesses
- current Guard status
- commands that actually ran and whether they succeeded
- pending approvals or warnings
- the next safe command, if one is needed

Do not claim that Guard replaced native sandboxing or host security. Do not claim
protection that the local status or command output did not verify.

## Reference

Current HOL Guard local setup and troubleshooting flow:
`https://github.com/hashgraph-online/hol-guard/blob/main/docs/guard/get-started.md`
