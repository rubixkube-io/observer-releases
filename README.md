<p align="center">
  <img src="https://rubixkube.ai/logo.svg" alt="RubixKube" width="60" />
</p>

<h1 align="center">RubixKube Observer Agent</h1>

<p align="center">
  <strong>Lightweight infrastructure observer for VMs and cloud environments</strong>
</p>

<p align="center">
  <a href="https://github.com/rubixkube-io/observer-releases/releases/latest"><img src="https://img.shields.io/github/v/release/rubixkube-io/observer-releases?style=flat-square&color=blue" alt="Latest Release"></a>
  <a href="https://github.com/rubixkube-io/observer-releases/releases/latest"><img src="https://img.shields.io/github/downloads/rubixkube-io/observer-releases/total?style=flat-square&color=green" alt="Downloads"></a>
  <a href="https://docs.rubixkube.ai"><img src="https://img.shields.io/badge/docs-rubixkube.ai-blue?style=flat-square" alt="Documentation"></a>
  <a href="https://rubixkube.ai"><img src="https://img.shields.io/badge/platform-rubixkube.ai-purple?style=flat-square" alt="Platform"></a>
</p>

---

The **Observer Agent** continuously monitors your infrastructure — VMs, AWS accounts, and GCP projects — and streams real-time insights to the [RubixKube](https://rubixkube.ai) platform. It detects anomalies in CPU, memory, disk, network, and cloud services, so RubixKube's AI agents can diagnose root causes and recommend fixes before customers are impacted.

## Quick Install

Get your API key from [console.rubixkube.ai](https://console.rubixkube.ai) → **Clusters** → **Add Cluster**, then run:

```bash
curl -fsSL https://docs.rubixkube.ai/install.sh | sudo bash
```

The interactive installer auto-detects your environment and walks you through setup. Pass `--api-key=<KEY>` to skip the prompt.

## Supported Platforms

| Platform | What It Monitors | Deployment |
|----------|-----------------|------------|
| **VM / Linux** | CPU, memory, disk, network, processes, swap | Binary + systemd service |
| **AWS** | EC2, EBS, RDS, Lambda, CloudWatch, IAM, S3, VPC | Binary on existing host _or_ new EC2 instance |
| **GCP** | Compute Engine, Cloud SQL, Cloud Functions, GCS, Cloud Monitoring | Docker on existing host _or_ new Compute Engine instance |

## How It Works

```
Your Infrastructure
  └─ Observer Agent (lightweight, single binary)
       ├─ Collects system metrics & cloud inventory
       ├─ Scores anomalies locally (CPU > 85%, disk > 90%, etc.)
       ├─ Deduplicates & batches insights
       └─ Streams to RubixKube via NATS
            └─ AI agents analyze, correlate, and generate RCA reports
```

The agent runs as a **systemd service** (VM/AWS) or **Docker container** (GCP), consuming minimal resources (~20 MB for VM, ~100 MB for AWS, ~200 MB for GCP with cloud CLIs included).

## Download Binaries

Pre-built binaries for each release are available on the [Releases](https://github.com/rubixkube-io/observer-releases/releases) page.

| Binary | Architecture | Platform |
|--------|-------------|----------|
| `vm-observer-agent-linux-amd64` | x86_64 | Linux |
| `vm-observer-agent-linux-arm64` | ARM64 / aarch64 | Linux |
| `vm-observer-agent-darwin-amd64` | x86_64 | macOS (development) |
| `vm-observer-agent-darwin-arm64` | Apple Silicon | macOS (development) |

### Manual Download

```bash
# Latest release — Linux amd64
curl -fsSL https://github.com/rubixkube-io/observer-releases/releases/latest/download/vm-observer-agent-linux-amd64 \
  -o /usr/local/bin/rk-observer
chmod +x /usr/local/bin/rk-observer
```

## Docker Images

Platform-specific container images are published to GitHub Container Registry:

```bash
# VM monitoring (minimal ~20MB)
docker pull ghcr.io/rubixkube-io/observer-agent:vm-latest

# AWS monitoring (includes AWS CLI ~100MB)
docker pull ghcr.io/rubixkube-io/observer-agent:aws-latest

# GCP monitoring (includes gcloud CLI ~200MB)
docker pull ghcr.io/rubixkube-io/observer-agent:gcp-latest
```

## Configuration

The agent is configured via environment variables. Key settings:

| Variable | Default | Description |
|----------|---------|-------------|
| `API_KEY` | — | RubixKube API key *(required)* |
| `CLUSTER_TYPE` | `vm` | Platform: `vm`, `aws`, or `gcp` |
| `SYSTEM_POLL_INTERVAL` | `30s` | Metrics collection interval |
| `CPU_THRESHOLD` | `85` | CPU usage % to trigger insight |
| `MEMORY_THRESHOLD` | `85` | Memory usage % to trigger insight |
| `DISK_THRESHOLD` | `90` | Disk usage % to trigger insight |
| `AWS_REGION_MODE` | `auto` | `auto` (discover all) or `explicit` |
| `GCP_PROJECT_IDS` | — | Comma-separated GCP project IDs |

## Uninstall

```bash
# Interactive uninstall
curl -fsSL https://docs.rubixkube.ai/install.sh | sudo bash -s -- --uninstall

# Or if already installed
sudo rk-observer --uninstall  # coming soon
```

The uninstaller cleans up binaries, systemd services, Docker containers, and optionally cloud resources (IAM roles, firewall rules, NAT gateways, service accounts).

## Requirements

- **VM**: Linux (amd64 or arm64), systemd, root access
- **AWS**: AWS CLI configured, IAM permissions for read-only access to EC2, RDS, Lambda, CloudWatch, S3, VPC
- **GCP**: `gcloud` CLI authenticated, IAM roles for read-only access to Compute, SQL, Functions, Monitoring

## Documentation

Full documentation is available at [docs.rubixkube.ai](https://docs.rubixkube.ai).

## About RubixKube

[RubixKube](https://rubixkube.ai) is an AI-native **Site Reliability Intelligence** platform. AI agents that watch, plan, act, and learn — 24/7. Keep revenue online.

- **Detect** anomalies across your entire stack
- **Diagnose** root causes with AI-powered analysis
- **Resolve** incidents with human-in-the-loop guardrails
- **Learn** from every incident to prevent recurrence

---

<p align="center">
  <a href="https://rubixkube.ai">Website</a> · <a href="https://console.rubixkube.ai">Console</a> · <a href="https://docs.rubixkube.ai">Docs</a> · <a href="mailto:connect@rubixkube.io">Contact</a>
</p>
