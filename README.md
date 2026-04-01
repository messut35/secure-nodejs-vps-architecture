![Architecture Diagram](secure-nodejs-vps-architecture-diagram.png)
# Secure Node.js VPS Architecture (Production Blueprint)

This repository documents a hardened VPS security architecture designed for production Node.js and Web3 backend services.

The environment follows a VPN-first administrative access model with centralized monitoring, intrusion detection visibility, and reverse proxy isolation.

---

## Architecture Overview

Core stack components:

- Ubuntu 24.04 hardened baseline
- WireGuard admin-only access plane (10.77.0.0/24)
- UFW default deny firewall posture
- Nginx reverse proxy segmentation
- Cloudflare origin protection
- PostgreSQL + Redis localhost-only binding
- PM2 Node.js production orchestration
- auditd security monitoring
- promtail → Loki → Grafana logging pipeline
- Telegram alert integration

## Threat Model Considerations

This architecture is designed to mitigate common production VPS risks:

- SSH brute-force attacks
- exposed administrative dashboards
- database lateral movement risks
- reverse proxy misconfiguration exposure
- privilege escalation visibility gaps
- silent service tampering

Administrative interfaces are isolated behind a WireGuard private access plane to reduce public attack surface.
---

## Access Control Model

Administrative access:

SSH restricted to WireGuard network only

Private services:

Grafana dashboards  
Admin panels  
POS interfaces  

No direct administrative surface exposed to the internet.

Public services:

Node.js APIs behind Nginx reverse proxy

---

## Monitoring Pipeline

auditd → promtail → Loki → Grafana → Telegram alerts

Detection coverage includes:

- privilege escalation attempts
- SSH configuration changes
- suspicious binary execution
- WireGuard configuration tampering
- reverse proxy anomalies

---

## Network Hardening Controls

Firewall posture:

default deny inbound

Administrative isolation:

WireGuard-only access plane

Service exposure:

localhost-bound databases

Minimal open port footprint

---

## Reverse Proxy Security Layer

Nginx reverse proxy enforces:

- isolation between public and private services
- Cloudflare origin protection
- TLS via DNS challenge certificates
- upstream exposure minimization

## Administrative Plane Isolation Strategy

Administrative access is intentionally separated from public traffic using a dedicated WireGuard network:

Public traffic flow:

Internet → Cloudflare → Nginx → Node.js services

Administrative traffic flow:

Operator device → WireGuard tunnel (10.77.0.0/24) → private dashboards

This ensures:

- no exposed SSH on public interface
- no exposed admin dashboards
- reduced attack surface for privilege escalation attempts
- safer production operations for solo-operator infrastructure environments
---

## Security Posture Summary

External exposure surface: LOW  
Administrative isolation strength: STRONG  
Realtime monitoring maturity: ADVANCED  

Production-grade hardened single-node architecture suitable for:

- Node.js backend services
- Web3 payment backends
- self-hosted SaaS infrastructure
- solo operator production stacks

---

## Architecture Report

See:

security-architecture-report.pdf

---

## Author

Masud Suhandi  
Node.js Infrastructure & Web3 Backend Engineer
