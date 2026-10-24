# Packet Tracer - Troubleshoot Connectivity Issues

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-CCNA-005691?style=for-the-badge&logo=cisco)
![Status](https://img.shields.io/badge/Status-Completed%20100%25-brightgreen?style=for-the-badge)

## 📌 Project Overview
This repository contains the walkthrough and solution for the **Cisco Packet Tracer - Troubleshoot Connectivity Issues** activity.

The objective was to identify, troubleshoot, and document network layer (IP/Gateway) and application layer (DNS) misconfigurations across local host machines attempting to access the external web server `www.cisco.pka`.

---

## 🗺️ Network Addressing Table

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | G0/0 | `172.16.1.1` | `255.255.255.0` | N/A |
| **R1** | G0/1 | `172.16.2.1` | `255.255.255.0` | N/A |
| **R1** | S0/0/0 | `209.165.200.226` | `255.255.255.252` | N/A |
| **R2** | G0/0 | `209.165.201.1` | `255.255.255.224` | N/A |
| **R2** | S0/0/0 (DCE) | `209.165.200.225` | `255.255.255.252` | N/A |
| **PC-01** | NIC | `172.16.1.3` | `255.255.255.0` | `172.16.1.1` |
| **PC-02** | NIC | `172.16.1.4` | `255.255.255.0` | `172.16.1.1` |
| **PC-A** | NIC | `172.16.2.3` | `255.255.255.0` | `172.16.2.1` |
| **PC-B** | NIC | `172.16.2.4` | `255.255.255.0` | `172.16.2.1` |
| **Web** | NIC | `209.165.201.2` | `255.255.255.224` | `209.165.201.1` |
| **DNS1** | NIC | `209.165.201.3` | `255.255.255.224` | `209.165.201.1` |
| **DNS2** | NIC | `209.165.201.4` | `255.255.255.224` | `209.165.201.1` |

---

## 🔍 Troubleshooting Steps & Findings

### 🔵 Part 1: PC-01 Analysis
* **Initial State:** IP/Subnet correct; Default Gateway correctly set to `172.16.1.1`.
* **Ping Tests:**
  * Gateway (`172.16.1.1`): **Successful**
  * Web Server (`209.165.201.2`): **Successful**
  * Local PCs (`PC-02`, `PC-A`, `PC-B`): **Successful**
* **Web Browser Tests:**
  * Via IP (`209.165.201.2`): **Successful**
  * Via Domain Name (`www.cisco.pka`): **Failed** (No DNS Server configured on PC-01).
* **Solution:** Set **Primary DNS** on PC-01 to `209.165.201.3` (DNS1).

---

### 🟢 Part 2: PC-02 Analysis
* **Initial State:** IP address incorrectly configured (wrong subnet/host portion or missing gateway).
* **Ping Tests:**
  * Gateway (`172.16.1.1`): **Failed** (before correction) / **Successful** (after correction).
  * Web Server & Local Hosts: **Successful** (after IP correction).
* **Web Browser Tests:**
  * Via IP (`209.165.201.2`): **Successful**
  * Via Domain Name (`www.cisco.pka`): **Failed** initially due to missing DNS entry.
* **Solution:**
  1. Correct IP configuration: IP `172.16.1.4`, Subnet `255.255.255.0`, Gateway `172.16.1.1`.
  2. Configure DNS Server: `209.165.201.3`.

---

### 🟡 Part 3: PC-A Analysis
* **Initial State:** Default Gateway incorrectly assigned (pointing to wrong interface/subnet).
* **Ping Tests:**
  * Gateway (`172.16.2.1`): **Successful** (after fix).
  * Web Server (`209.165.201.2`): **Successful** (after fix).
  * Local PCs (`PC-B`, `PC-01`, `PC-02`): **Successful** (after fix).
* **Web Browser Tests:**
  * Via IP (`209.165.201.2`): **Successful**
  * Via Domain Name (`www.cisco.pka`): **Successful** (after DNS configuration).
* **Solution:**
  1. Change Default Gateway to `172.16.2.1`.
  2. Assign DNS Server `209.165.201.3`.

---

### 🔴 Part 4: PC-B Analysis & Escallation
* **Initial State:** IP configuration updated; DNS pointed to secondary DNS (`209.165.201.4` - DNS2).
* **Ping Tests:**
  * Gateway (`172.16.2.1`): **Successful**
  * Web Server (`209.165.201.2`): **Successful**
  * Local PCs: **Successful**
* **Web Browser Tests:**
  * Via IP (`209.165.201.2`): **Successful**
  * Via Domain Name (`www.cisco.pka`): **Failed** when using DNS2 (`209.165.201.4`).
* **Solution & Escalation:**
  * Temporary fix: Change DNS to DNS1 (`209.165.201.3`).
  * **Escalation required:** DNS2 server (`209.165.201.4`) is missing the A-record entry for `www.cisco.pka` or the DNS service is inactive on DNS2. Escalate to the Server/ISP Administrator to update the zone database on DNS2.

---

## ✅ Completion Criteria
* All hosts (`PC-01`, `PC-02`, `PC-A`, `PC-B`) have valid IP addressing and default gateways matching the Addressing Table.
* All host PCs can resolve `www.cisco.pka` using DNS1 (`209.165.201.3`).
* Activity Completion: **100%**.