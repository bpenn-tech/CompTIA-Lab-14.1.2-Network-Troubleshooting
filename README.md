# CompTIA-Lab-14.1.2-Network-Troubleshooting
Diagnosed and resolved network connectivity failure on Windows 11 workstation by identifying incorrect static IP configuration using ping diagnostics and IPv4 analysis. Scored 100%. CompTIA A+ Core 2 Lab 14.1.2
# Lab 14.1.2: Assisted Network Troubleshooting

## Lab Overview
**Course:** CompTIA A+ Core 2
(220-1202) CertMaster Perform V15
**Lab Number:** 14.1.2
**Topic:** Network Connectivity
Troubleshooting
**Date Completed:** May 2026
**Score:** 100% 🏆

---

## Scenario
A work laptop was recently brought
home and connected to a small office
home office (SOHO) network. The laptop
was unable to access the internet.
My task was to systematically
identify and resolve the
connectivity issue using the
CompTIA troubleshooting methodology.

---

## Network Topology
The home network consisted of:
- Router (gateway: 192.168.1.1)
- Switch connecting all devices
- Home-PC1 (192.168.1.21)
- Home-PC2 (192.168.1.22)
- Home-Laptop (problem device)

![Network Topology]
(screenshots/02-network-topology-diagram.png)

---

## Environment
- Simulation: CompTIA
  CertMaster Perform V15
- Lab: 14.1.2 Assisted
  Network Troubleshooting
- Devices Used:
  - Home-Laptop (problem device)
  - Home-PC1 (reference device)
  - Home-PC2 (reference device)
- Tools Used:
  - Network & Internet Settings
  - IPv4 Properties
  - Windows PowerShell
  - ping command
  - ipconfig

---

## Troubleshooting Methodology

### Step 1: Gather Information
#### Home-Laptop IP Settings

Opened Network & Internet settings
and navigated to IPv4 Properties
on Home-Laptop to examine
current configuration.

![Home-Laptop Wrong IPv4 Settings]
(screenshots/03-homelaptop-ipv4-wrong-settings.png)

**Findings:**
- IP Address: 192.169.1.20
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DNS: 192.168.1.1

**Immediate Red Flag:**
Windows displayed a warning
that the default gateway was
not on the same network segment!

![Gateway Warning]
(screenshots/04-homelaptop-gateway-warning.png)

---

### Step 2: Identify Symptoms
#### Ping Tests from Home-Laptop

Ran ping tests from Home-Laptop
to identify exactly where
connectivity was failing.

**ping Home-Laptop (self):**
PASSED ✓ - TCP/IP stack working!
![Ping Self Pass]
(screenshots/06-homelaptop-ping-self-pass.png)

**ping Home-PC1:**
FAILED ✗ - 100% packet loss!
![Ping Home-PC1 Fail]
(screenshots/07-homelaptop-ping-homepc1-fail.png)

**ping Home-PC2:**
FAILED ✗ - 100% packet loss!
![Ping Home-PC2 Fail]
(screenshots/08-homelaptop-ping-homepc2-fail.png)

**ping 192.168.1.1 (Router):**
FAILED ✗ - 100% packet loss!
![Ping Router Fail]
(screenshots/09-homelaptop-ping-router-fail.png)

**Pattern Identified:**
Home-Laptop could only
reach itself! Could not
reach any other device
on the network!

---

### Step 3: Gather Information
#### Home-PC1 IP Settings

Switched to Home-PC1 to compare
settings with the broken laptop.

![Home-PC1 IPv4 Settings]
(screenshots/13-homepc1-ipv4-settings.png)

**Home-PC1 Settings:**
- IP Address: 192.168.1.21 ✓
- Subnet Mask: 255.255.255.0 ✓
- Default Gateway: 192.168.1.1 ✓
- DNS: 192.168.1.1 ✓

---

### Step 4: Identify Symptoms
#### Ping Tests from Home-PC1

**ping Home-PC1 (self):**
PASSED ✓
![PC1 Ping Self]
(screenshots/12-homepc1-ping-self-pass.png)

**ping Home-Laptop:**
FAILED ✗ - Laptop unreachable!
![PC1 Ping Laptop Fail]
(screenshots/14-homepc1-ping-homelaptop-fail.png)

**ping Home-PC2:**
PASSED ✓ - Same network!
![PC1 Ping PC2 Pass]
(screenshots/15-homepc1-ping-homepc2-pass.png)

**ping 192.168.1.1 (Router):**
PASSED ✓ - Can reach gateway!
![PC1 Ping Router Pass]
(screenshots/16-homepc1-ping-router-pass.png)

---

### Step 5: Theory of Probable Cause

#### Root Cause Identified! 🎯

**Critical Comparison:**

| Device | IP Address | Network |
|--------|-----------|---------|
| Home-Laptop | 192.169.1.20 | 192.169.1.0 ✗ |
| Home-PC1 | 192.168.1.21 | 192.168.1.0 ✓ |
| Home-PC2 | 192.168.1.22 | 192.168.1.0 ✓ |
| Router | 192.168.1.1 | 192.168.1.0 ✓ |

**Theory:**
Home-Laptop was configured
with incorrect IP address
192.169.1.20 instead of
the correct 192.168.x.x
home network range!

The laptop still had its
work network IP settings!
One digit difference
(169 vs 168) placed it
on a completely different
network unable to communicate
with any home network devices!

![Questions Answered]
(screenshots/17-questions-8-9-answered.png)

---

### Step 6: Implement Solution

Changed Home-Laptop IP address
from 192.169.1.20 to 192.168.1.20

**One digit change fixed
the entire problem!**

169 → 168

![Solution Implemented]
(screenshots/18-solution-implemented.png)

---

### Step 7: Verify Full Functionality

Ran all ping tests again
from Home-Laptop after fix!

**ping Home-Laptop (self):**
PASSED ✓
![Verify Self]
(screenshots/19-verification-ping-self-pass.png)

**ping Home-PC1:**
PASSED ✓
![Verify PC1]
(screenshots/20-verification-ping-homepc1-pass.png)

**ping Home-PC2:**
PASSED ✓
![Verify PC2]
(screenshots/21-verification-ping-homepc2-pass.png)

**ping 192.168.1.1 (Router):**
PASSED ✓
![Verify Router]
(screenshots/22-verification-ping-router-pass.png)

**All tests passing!
Internet access restored! 🎉**

---

## Root Cause Summary
PROBLEM:
Home-Laptop IP: 192.169.1.20
Home Network:   192.168.1.0/24
One digit difference!
.169 vs .168
Laptop was on wrong
network entirely!
SOLUTION:
Changed IP to: 192.168.1.20
Now on correct network!
All connectivity restored! ✓

---

## Key Lessons Learned
Always check IP settings
first when troubleshooting
network connectivity!
Compare broken device
with working device -
differences reveal
the problem!
"Request timed out"
means packet sent but
no response received!
Often means wrong network
or firewall blocking!
One wrong digit in
IP address can completely
isolate a device!
Laptops brought from
work to home often have
wrong IP settings!
Always check configuration
when changing networks!
Ping tests from bottom
up (OSI model) help
isolate exactly where
connectivity breaks!

---

## Commands Used

```powershell
ping Home-Laptop
ping Home-PC1
ping Home-PC2
ping 192.168.1.1
```

---

## CompTIA A+ Objectives Covered
Core 2 (220-1202):
→ 5.7 Given a scenario,
troubleshoot common
wired and wireless
network problems
Core 1 (220-1201):
→ 5.1 Given a scenario,
troubleshoot common
problems associated
with networking

---

## Lab Completion

![Final Score 100%]
(screenshots/23-final-score-100percent.png)

**Score: 100%** 🏆

---

## Certification Progress
- CompTIA A+ Core 1 (220-1201)
  → In Progress
- CompTIA A+ Core 2 (220-1202)
  → In Progress
- Per Scholas AI-Enabled
  IT Support Program
  → Currently Enrolled
- Google IT Support Certificate
  → Complete
- Google AI Essentials Certificate
  → Complete
