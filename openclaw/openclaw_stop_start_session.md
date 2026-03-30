# OpenClaw Stop/Start Session (Lab Profile)

Use this when you want to stop OpenClaw for the night and start again tomorrow.

---

## Stop OpenClaw tonight

Run:

```bash
systemctl --user stop openclaw-gateway-lab.service || true
systemctl --user status openclaw-gateway-lab.service || true
```

If you also want to prevent it from auto-starting next time:

```bash
systemctl --user disable openclaw-gateway-lab.service || true
```

Since your healthy setup is on the lab profile, `openclaw-gateway-lab.service` is the right one to stop.

---

## Start OpenClaw tomorrow

Run:

```bash
systemctl --user start openclaw-gateway-lab.service
systemctl --user status openclaw-gateway-lab.service || true
openclaw --profile lab status
openclaw --profile lab health
```

If you disabled it tonight, this still works because `start` launches it manually.

---

## Continue your session

Then continue with either:

```bash
openclaw --profile lab dashboard --no-open
```

or:

```bash
openclaw --profile lab tui
```

---

## Quick routine for tomorrow

1. start service
2. check service status
3. check profile status
4. check profile health
5. open dashboard or TUI
6. continue using the `lab` profile
