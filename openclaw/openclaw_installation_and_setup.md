# OpenClaw Installation and Setup Runbook

## Purpose

This runbook covers the path to get OpenClaw installed, onboarded, validated, and ready for use.

---

## Environment assumptions

This runbook assumes:

- Linux host
- user is operating as `root`
- Node.js and npm are already available
- systemd user services are available
- OpenClaw is being run locally on the host
- remote dashboard access may later be done through SSH port forwarding

---

# Part 1 — Installation

## 1. Install OpenClaw globally

```bash
npm install -g openclaw
```

## 2. Verify installation

```bash
openclaw --version
which openclaw
```

Expected outcome:
- `openclaw --version` prints a valid version
- `which openclaw` prints the installed binary path

---

# Part 2 — Initial setup and bootstrap

Use an isolated profile so the setup does not collide with default or old state.

## 1. Start onboarding

```bash
openclaw --profile lab onboard --install-daemon
```

---

## 2. Wizard choices

Use the following choices during onboarding.

### Security acknowledgement
Choose:

- `Yes`

### Setup mode
Choose:

- `QuickStart`

### Gateway/networking
Keep it minimal:

- bind to loopback / localhost only
- token auth
- no Tailscale
- local-only exposure

### Model/auth provider
Use one provider only for the first clean bootstrap.

Recommended principle:
- choose a single supported provider
- do not configure multiple auth methods at this stage

### Channels
Choose:

- `Skip for now`

Reason:
- channels are not needed to validate base health
- adding them now increases troubleshooting surface

### Search provider
Choose:

- `Skip for now`

Reason:
- web search is optional
- skip all external API dependencies during first bootstrap

### Skills
Choose:

- `No`

Reason:
- skills add dependencies and setup noise
- they are not required to prove the gateway is healthy

### Hooks
Choose:

- `Skip for now`

Reason:
- hooks add automation behavior
- not needed for a clean base milestone

### Gateway service already installed
If prompted with something like:

- `Gateway service already installed`

Choose:

- `Reinstall`

Reason:
- this is safer than restart when aiming for a truly fresh bootstrap

### Start TUI / Open Web UI / Later
Choose:

- `Do this later`

Reason:
- first validate `status` and `health`
- then move into TUI or Web UI afterward

---

# Part 3 — Validation after onboarding

As soon as onboarding finishes, run:

```bash
openclaw --profile lab status
openclaw --profile lab health
```

## Healthy result criteria

### `status` should show:

- gateway reachable
- gateway service installed, enabled, running
- local loopback endpoint
- no unauthorized errors
- no device-token mismatch
- zero sessions is acceptable before first interaction

### `health` should return cleanly

A healthy output can look like:
- default agent shown
- heartbeat interval shown
- session store path shown
- no auth or gateway-closure error

---

# Part 4 — First dashboard access

After `status` and `health` are clean, you can get dashboard info:

```bash
openclaw --profile lab dashboard --no-open
```

If the host is remote, use SSH port forwarding from your local machine:

```bash
ssh -N -L 18789:127.0.0.1:18789 root@YOUR_HOST_IP
```

Then open locally in your browser:

```text
http://localhost:18789/
```

If you need the tokenized URL, copy it from onboarding or dashboard output.

---

# Part 5 — Milestone reached

The setup is considered healthy when all of the following are true:

- OpenClaw is installed
- onboarding completes successfully
- gateway service is running
- gateway is reachable
- `openclaw --profile lab status` is healthy
- `openclaw --profile lab health` returns cleanly
- no `device token mismatch`
- no `unauthorized` close during health checks

---

# Part 6 — Compact install + bootstrap sequence

```bash
npm install -g openclaw
openclaw --version
openclaw --profile lab onboard --install-daemon
openclaw --profile lab status
openclaw --profile lab health
```

---

# Part 7 — Success checklist

Use this checklist after bootstrap:

- [ ] `openclaw --version` works
- [ ] `openclaw --profile lab onboard --install-daemon` completes
- [ ] `openclaw --profile lab status` shows gateway reachable
- [ ] `openclaw --profile lab health` returns cleanly
- [ ] no `device token mismatch`
- [ ] no gateway unauthorized close
- [ ] service is active
- [ ] dashboard is available locally
