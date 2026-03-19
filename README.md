# Automated Cyber Range

Ansible roles and Ludus range config to deploy a full SOC lab environment automatically.

## What gets deployed

| VM | Roles | VLAN | IP |
|----|-------|------|----|
| Wazuh server | `wazuh_server` | 10 | `.10` |
| TheHive | `thehive` | 10 | `.11` |
| Cortex | `docker` + `cortex` | 10 | `.12` |
| MISP | `misp` | 10 | `.13` |
| Shuffle | `docker` + `shuffle` | 10 | `.14` |
| DVWA | `wazuh_agent` + `dvwa` | 20 | `.10` |


---

## Requirements

| Requirement | Version |
|-------------|---------|
| Proxmox | 9.0+ |
| Ludus | latest |
| Ansible | 2.9+ |
| Python | 3.8+ |

**Ludus templates required:**
- `kali-x64-desktop-template` -- used by Wazuh server
- `ubuntu-24.04-x64-server-template` -- used by all other VMs

**Minimum host resources:** 32 GB RAM · 12 CPU cores

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/yefimbas/automated-cyber-range.git
cd automated-cyber-range
```

### 2. Add roles to Ludus

```bash
ludus ansible role add -d ./roles/wazuh_server
ludus ansible role add -d ./roles/wazuh_agent
ludus ansible role add -d ./roles/thehive
ludus ansible role add -d ./roles/cortex
ludus ansible role add -d ./roles/misp
ludus ansible role add -d ./roles/shuffle
ludus ansible role add -d ./roles/docker
ludus ansible role add -d ./roles/dvwa
```

### 3. Set the range config

```bash
ludus range config set -f range_config.yml
```

### 4. Deploy

```bash
ludus range deploy
```

Follow logs in real time:

```bash
ludus range logs -f
```

---

## Configuration

### Role variables

| Variable | VM | Required | Description |
|----------|----|----------|-------------|
| `wazuh_admin_password` | wazuh_server | ❌ | If set, overrides the default Wazuh admin password after install |
| `wazuh_server_ip` | dvwa (wazuh_agent) | ✅ | IP of the Wazuh server — set automatically via `range_config.yml` |
| `wazuh_agent_name` | dvwa (wazuh_agent) | ❌ | Agent display name in Wazuh (default: `ludus-agent`) |



---

## License

GPL-3.0