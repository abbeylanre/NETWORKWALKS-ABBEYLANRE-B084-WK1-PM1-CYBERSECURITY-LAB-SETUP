
# 🛡️ Cybersecurity & Pentesting Lab Setup

> **Isolated Virtual Environment Baseline | Project Documentation**

---

## 📌 1. Project Overview & Objectives

This project establishes a isolated virtual lab environment using **VirtualBox** and **Kali Linux** to support safe, repeatable cybersecurity testing, network scanning, vulnerability assessment, and ethical hacking exercises.

* **Core Purpose:** Provide a sandboxed environment where security-testing tools can be deployed without risking host systems or unauthorized networks.
* **Scalability:** Configured on a private subnetwork to seamlessly support future target VMs.

### Key Objectives Checklist

* [x] Deploy and configure VirtualBox Hypervisor.
* [x] Import Kali Linux guest environment.
* [x] Provision a custom NAT Network infrastructure (`10.0.0.0/24`).
* [x] Assign static IPv4 configurations & verify DNS resolution.
* [x] Establish a clean VM base snapshot for system recovery.
* [x] Document configuration, verification steps, and troubleshooting workflows.

---

## ⚙️ 2. Environment Specifications

### 🖥️ Hardware & Software Specs

| Component | System Configuration | Notes / Version |
| --- | --- | --- |
| **Host OS** | Windows 10 | Base System |
| **Processor & Memory** | Intel Core i7 | 8 GB RAM | Host Hardware Allocation |
| **Hypervisor** | VirtualBox | Version 7.2 |
| **Security Guest OS** | Kali Linux | Version 2026.2 (2048 MB RAM) |
| **Compression Utility** | 7-Zip | Archive extraction |

### 🌐 Network Topography

| Parameter | Setting | Details |
| --- | --- | --- |
| **Network Type** | NAT Network | Inter-VM + Internet Connectivity |
| **Network Name** | `NatNetwork` | VirtualBox Network Profile |
| **Subnet Mask / Prefix** | `10.0.0.0/24` | IPv4 CIDR Block (DHCP Enabled) |
| **Kali VM Static IP** | `10.0.0.2` | Subnet mask: `255.255.255.0` |
| **Gateway & DNS** | `10.0.0.1` (Gateway) | Primary DNS: `8.8.8.8` (Google) |
| **Target Allocation Range** | `10.0.0.3` – `10.0.0.99` | Reserved for future targets/victims |

---

## 🪜 3. Implementation Workflow

### Step 1: Utility Installation

* Installed **7-Zip** to handle archive extraction (`.7z`) of virtual machine images.
* Installed **VirtualBox 7.2** hypervisor.
#### Practical Evidence

![VirtualBox Interface](Week%201%20Screenshots/Screenshot%20virtualbox%20interface.png)

### Step 2: Virtual Network Provisioning

* Built a custom **NAT Network** (`NatNetwork`) with CIDR `10.0.0.0/24`.
* *Design Rationale:* Unlike default NAT (which isolates guests from each other), a NAT Network allows inter-machine communication between attacker and victim VMs while sharing outbound internet access.
#### Practical Evidence

![Network Configuration](Week%201%20Screenshots/Screenshot%20network%20settings.png)

### Step 3: VM Import & Hardware Setup

* Imported Kali Linux appliance into VirtualBox.
* Set **Adapter 1** to *NAT Network* (`NatNetwork`) using the **Intel PRO/1000 MT Desktop** emulator.
* Allocated **2048 MB RAM** and established host-to-guest shared folders.
#### Practical Evidence

![Kali Linux Deployment](Week%201%20Screenshots/Screenshot%20kali%20linux.png)

### Step 4: Network Addressing & Baselining

* Configured Kali Linux with a predictable static IP: `10.0.0.2/24`.
* Created baseline snapshot: `Clean Kali - Network Setup` to preserve a pristine state before security operations.
#### Practical Evidence

![Kali Linux Network Settings](Week%201%20Screenshots/Screenshot%20kali%20network%20settings.png)

---

## 🔎 4. Verification & Validation Matrix

| Test Objective | Execution Command | Success Criteria | Result |
| --- | --- | --- | --- |
| **Interface Verification** | `ip a` | `10.0.0.2/24` correctly assigned | **PASSED** |
| **Gateway Reachability** | `ping 10.0.0.1` | 0% packet loss to virtual gateway | **PASSED** |
| **Outbound Internet IP** | `ping 8.8.8.8` | Successful ICMP replies | **PASSED** |
| **DNS Resolution** | `nslookup networkwalks.com` | Domain successfully resolved | **PASSED** |
| **Core Tooling** | `nmap --version` | Valid version header returned | **PASSED** |
| **State Recovery** | *Snapshot Restore Test* | Baseline network state restored | **PASSED** |
#### Practical Evidence

![Network Connectivity Test](Week%201%20Screenshots/Screenshot%20connectivity%20test.png)

---

## 🐞 5. Troubleshooting Log

### Case 1: Post-Static IP Internet Connectivity Drop

* **Symptom:** Network connectivity became unresponsive following manual IPv4 addressing.
* **Root Cause:** NetworkManager Duplicate Address Detection (DAD) timeout conflicts on the interface.
* **Mitigation:** Executed interface tweak via `nmcli`:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

```


*(Note: Verify local interface identity with `nmcli connection show` prior to modification).*

### Case 2: VirtualBox VT-x / Firmware Execution Failure

* **Symptom:** VM launch failed immediately with a virtualization state error.
* **Root Cause:** Hardware Virtualization (Intel VT-x / AMD-V) was disabled in host UEFI/BIOS firmware.
* **Mitigation:** Rebooted host → Entered UEFI Setup → Enabled **Intel VT-x** → Saved and booted.

---

## 💡 6. Core Competencies Gained

1. **Virtual Network Architecture:** Understanding the structural and operational differences between isolated host-only, default NAT, and communal NAT Networks.
2. **Network Protocol Configuration:** Hands-on manual setup of IPv4 subnets, default gateways, and DNS resolvers in Linux.
3. **Hypervisor Resilience Strategy:** Utilizing immutable base snapshots to safeguard against system degradation during penetration testing.
4. **Technical Documentation Standards:** Recording infrastructure blueprints, problem-solving workflows, and validation records for auditing.

---

## 🔐 Security & Disclaimer

> **Notice:** This environment is constructed exclusively for authorized security education, research, and skill development.

---

## 👤 Author Metadata

* **Author:** Abiodun Omotosho ([LinkedIn](https://www.linkedin.com/in/abiodunomotosho1))
* **Program:** Networkwalks Cybersecurity Internship
* **Milestone:** Week 01 – Cybersecurity & Pentesting Lab Setup
* **Repository:** [GitHub Repository](https://github.com/abbeylanre/NETWORKWALKS-CYBERSECURITY-INTERNSHIP)
