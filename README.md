# Cloud Security Platform (OCI)

## Overview

This project demonstrates the design and implementation of a hardened SSH intrusion detection and response platform deployed on Oracle Cloud Infrastructure (OCI).

The system monitors authentication activity in real time, detects suspicious login attempts, enriches IP intelligence using AbuseIPDB, and escalates alerts through Telegram.

The objective of this project is to simulate a lightweight, security-focused cloud monitoring system using disciplined infrastructure hardening principles.

---

## Project Structure

cloud-security-platform/
├── architecture/        → Design documentation and threat model
├── docs/                → Deployment guide
├── systemd/             → Hardened service definition
├── oci-auth-monitor/    → Python monitoring application
├── docker/              → Containerization support (future expansion)

---

## Key Security Controls

- SSH key-based authentication only
- Root login disabled
- rpcbind disabled
- UFW firewall enabled
- systemd service sandboxing
- Environment-based secret management
- Structured JSON logging
- Threat intelligence enrichment (AbuseIPDB)
- Alert cooldown logic to prevent fatigue

---

## Threat Model

See: architecture/threat-model.md

---

## Deployment Guide

See: docs/setup-guide.md

---

## Design Philosophy

- Minimize attack surface
- Enforce least privilege
- Detect early
- Log structurally
- Document clearly
- Keep architecture intentional

---

## Skills Demonstrated

- Linux system hardening (SSH, UFW, service isolation)
- systemd service configuration with sandboxing
- Real-time log monitoring using journalctl
- Regular expression log parsing
- Threat intelligence API integration (AbuseIPDB)
- Secure secret management using environment variables
- Rate limiting and alert fatigue control logic
- Structured JSON event logging
- Cloud VM security design (OCI)
- Version control using Git and GitHub workflow

---

## High-Level Architecture

Internet
   │
   ▼
[ Public OCI VM ]
   │
   ├── Hardened Ubuntu Server
   │     ├── SSH (key-only auth)
   │     ├── UFW Firewall
   │     └── Disabled unnecessary services
   │
   ├── systemd Service (auth-monitor)
   │     └── journalctl real-time log stream
   │
   ├── Detection Engine (Python)
   │     ├── Regex-based log parsing
   │     ├── Dynamic threshold logic
   │     ├── Rate limiting control
   │     └── JSON structured logging
   │
   ├── Threat Intelligence
   │     └── AbuseIPDB API enrichment
   │
   └── Alerting Layer
         └── Telegram notifications

---

## Architecture Diagram

```
                ┌──────────────────────────┐
                │        Internet          │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │      OCI Ubuntu VM       │
                │  (Public IP Assigned)    │
                ├──────────────────────────┤
                │  SSH (Key-Only Auth)     │
                │  UFW Firewall            │
                │  Disabled rpcbind        │
                ├──────────────────────────┤
                │  systemd: auth-monitor   │
                │  ├─ journalctl stream    │
                │  ├─ Regex detection      │
                │  ├─ Dynamic thresholds   │
                │  ├─ JSON logging         │
                │  └─ AbuseIPDB enrichment │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │      Telegram Alerts     │
                └──────────────────────────┘
```


## Example Attack Scenario

### Scenario: SSH Brute-Force Attempt

An automated bot begins attempting repeated SSH logins against the public IP of the OCI VM.

### What Happens Without Monitoring

- Failed login attempts accumulate silently
- No alert is generated
- Administrator is unaware of attack activity
- No visibility into source reputation

### What Happens With This Platform

1. The monitor detects repeated failed login patterns.
2. A dynamic threshold is reached (default: 3 attempts).
3. The IP is enriched using AbuseIPDB.
4. A Telegram alert is generated with:
   - Source IP
   - Attempt count
   - Abuse confidence score (if available)
   - Timestamp
5. The event is logged in structured JSON format for auditing.

This transforms raw SSH noise into actionable security intelligence.

---

---

## Sample Alert Output (Sanitized)

### Telegram Alert Example

```
⚠️ SSH ATTACK DETECTED ⚠️
IP: 185.220.101.45
Attempts: 3
Abuse Score: 92
Time: 2026-02-13 10:42:17 AM
```

### Structured JSON Log Entry

```
{
  "type": "ssh_attack_detected",
  "ip": "185.220.101.45",
  "count": 3,
  "timestamp": "2026-02-13T10:42:17-05:00"
}
```

---

### Successful Login Example

```
🚨 SSH LOGIN SUCCESS 🚨
User: admin
IP: 192.168.1.15
Host: oci-secure-vm
Time: 2026-02-13 09:02:11 AM
```


## Why I Built This

I built this project to move beyond theoretical security concepts and implement a practical, cloud-based intrusion detection system from scratch.

Rather than relying on prebuilt tools, I wanted to understand:

- How SSH authentication events are generated and logged
- How to monitor system logs in real time
- How brute-force attacks appear at the system level
- How to enrich events with external threat intelligence
- How to control alert fatigue through rate limiting
- How to deploy monitoring as a hardened systemd service

This project represents hands-on defensive infrastructure engineering in a live cloud environment.
