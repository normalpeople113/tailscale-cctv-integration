# 🎥 Tailscale CCTV Integration Pattern
> Secure, zero-config mesh networking for multi-site CCTV integration using Tailscale + Socat relay.

[![Pattern](https://img.shields.io/badge/Pattern-Mesh%20VPN%20Integration-blue)]()
[![Protocol](https://img.shields.io/badge/Protocol-RTSP%20over%20Tailscale-green)]()
[![License](https://img.shields.io/badge/License-MIT-lightgrey)]()

---

## 🎯 Problem Statement
Multi-site CCTV deployments face challenges:
- Devices behind NAT/firewall with no public IP
- NVR systems that only accept local-network RTSP streams
- Complex port-forwarding configurations that expose security risks
- Need for scalable, secure, and maintainable remote video access

## 💡 Core Concept
This pattern introduces a **zero-config mesh VPN layer** (Tailscale) combined with **lightweight TCP relay** (Socat) to:
- Expose local RTSP streams securely over encrypted tunnel
- Bridge remote NVR/VNVR systems without public IP or port forwarding
- Maintain modular, site-independent deployment
- Enable scalable expansion to 10+ sites with minimal configuration

## 🏗️ Architecture
```mermaid
flowchart LR
    subgraph SiteA["📍 Site A (Source)"]
        CCTV["📹 CCTV (RTSP)"] --> LTE["📡 LTE Router"]
        LTE --> GL["🔌 GL.iNet GL-MT300N-V2"]
        GL -->|Socat Relay| TS_A["🔐 Tailscale Client"]
    end

    subgraph Mesh["🌐 Tailscale Encrypted Mesh"]
        TS_A <-->|WireGuard| TS_B
    end

    subgraph SiteB["📍 Site B (Receiver)"]
        TS_B["🔐 Tailscale Client"] -->|Socat Relay| UB["🖥️ Ubuntu Server"]
        UB --> FR["🎬 Frigate/Shinobi VNVR"]
        UB --> NVR["📼 XMEye NVR (Local RTSP)"]
    end

    style CCTV fill:#1e3a5f,stroke:#00d4ff,color:#fff
    style GL fill:#1e3a8a,stroke:#3b82f6,color:#fff
    style TS_A fill:#064e3b,stroke:#10b981,color:#fff
    style TS_B fill:#064e3b,stroke:#10b981,color:#fff
    style UB fill:#14532d,stroke:#22c55e,color:#fff
    style NVR fill:#451a03,stroke:#f59e0b,color:#fff
