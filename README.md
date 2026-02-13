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
