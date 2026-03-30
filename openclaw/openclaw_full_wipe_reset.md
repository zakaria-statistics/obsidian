# OpenClaw Full Wipe / Reset Runbook

## Purpose

Use this runbook to fully wipe OpenClaw install/state and reset the host back to a clean slate.

---

## When to use this

Use this when setup is broken, contaminated, or stuck in a bad auth/device-token state.

---

# Part 1 — Full wipe / reset path

## 1. Stop and remove old gateway services

```bash
systemctl --user stop openclaw-gateway.service || true
systemctl --user disable openclaw-gateway.service || true
systemctl --user stop openclaw-gateway-lab.service || true
systemctl --user disable openclaw-gateway-lab.service || true

rm -f /root/.config/systemd/user/openclaw-gateway.service
rm -f /root/.config/systemd/user/openclaw-gateway-lab.service
rm -f /root/.config/systemd/user/default.target.wants/openclaw-gateway.service
rm -f /root/.config/systemd/user/default.target.wants/openclaw-gateway-lab.service

systemctl --user daemon-reload
systemctl --user reset-failed
```

## 2. Uninstall the global CLI

```bash
npm uninstall -g openclaw || true
pnpm remove -g openclaw || true
```

Notes:
- `pnpm` may not be installed, which is fine
- `|| true` prevents the flow from stopping if the package is absent

## 3. Remove all OpenClaw state, profiles, caches, and symlinks

```bash
rm -f /root/.clawdbot

rm -rf /root/.openclaw
rm -rf /root/.openclaw-lab
rm -rf /root/.openclaw-dev

rm -rf /root/.config/openclaw
rm -rf /root/.cache/openclaw
```

## 4. Verify wipe completion

Run these checks:

```bash
which openclaw || true
ls -la /root | grep -E 'openclaw|clawdbot' || true
systemctl --user status openclaw-gateway.service || true
systemctl --user status openclaw-gateway-lab.service || true
```

Expected clean results:

- `which openclaw` returns nothing
- `ls -la /root | grep -E 'openclaw|clawdbot'` returns nothing
- both `systemctl --user status ...` commands say unit not found

If anything still appears, remove the remaining file, symlink, or service and rerun the checks.

---

# Part 2 — Compact full wipe sequence

```bash
systemctl --user stop openclaw-gateway.service || true
systemctl --user disable openclaw-gateway.service || true
systemctl --user stop openclaw-gateway-lab.service || true
systemctl --user disable openclaw-gateway-lab.service || true

rm -f /root/.config/systemd/user/openclaw-gateway.service
rm -f /root/.config/systemd/user/openclaw-gateway-lab.service
rm -f /root/.config/systemd/user/default.target.wants/openclaw-gateway.service
rm -f /root/.config/systemd/user/default.target.wants/openclaw-gateway-lab.service

systemctl --user daemon-reload
systemctl --user reset-failed

npm uninstall -g openclaw || true
pnpm remove -g openclaw || true

rm -f /root/.clawdbot
rm -rf /root/.openclaw /root/.openclaw-lab /root/.openclaw-dev
rm -rf /root/.config/openclaw /root/.cache/openclaw

which openclaw || true
ls -la /root | grep -E 'openclaw|clawdbot' || true
systemctl --user status openclaw-gateway.service || true
systemctl --user status openclaw-gateway-lab.service || true
```
