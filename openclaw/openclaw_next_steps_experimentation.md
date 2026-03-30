# OpenClaw Next Steps and Experimentation Runbook

## Purpose

This runbook covers what to do after the base installation and health milestone is reached.

---

## Recommended next steps after bootstrap

After reaching a healthy gateway milestone, proceed in this order:

1. open dashboard or TUI
2. create the first simple session
3. test one basic interaction
4. review security warnings
5. add one feature at a time:
   - one search provider
   - one channel
   - selected skills
   - hooks

Do not enable many integrations at once.

---

## Experimentation notes

- Use `--profile lab` consistently during early testing
- keep channels, search, skills, and hooks disabled until the base runtime is proven healthy
- if the system is remote, keep the Control UI local-only and use SSH tunneling
- review security warnings before exposing the UI beyond localhost
