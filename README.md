# NetworkWalks Cybersecurity Lab — Week 1, Project Module 1

## Lab Overview

- **Lab:** Cybersecurity Testing Environment Setup
- **Platform:** NetworkWalks
- **Module:** Week 1, Project Module 1 (WK1-PM1)
- **Primary tool:** Oracle VirtualBox
- **Attacking machine:** Kali Linux
- **Focus areas:** Virtualisation, network configuration, Linux networking, and cybersecurity lab preparation

## 1. Objective

The objective of this lab is to establish a virtualised cybersecurity testing environment using VirtualBox and Kali Linux. The environment provides a dedicated network for future ethical hacking exercises, penetration-testing practice, and security assessments.

The lab covers configuring a custom NAT Network, assigning a static IP address to Kali Linux, enabling virtual-machine integration features, and preparing the environment for additional target machines.

## 2. Environment and Tools

| Component | Configuration |
|---|---|
| Host operating system | Windows 10, as specified in the NetworkWalks reference architecture |
| Virtualisation platform | Oracle VirtualBox |
| Attacking machine | Kali Linux |
| Virtual network | Custom NAT Network |
| Network subnet | `10.0.0.0/24` |
| Kali Linux IP address | `10.0.0.2/24` |
| Archive utility | 7-Zip |
| Network configuration | NetworkManager (`nmcli`) |
| Integration features | Shared folders, clipboard sharing, and drag-and-drop |

### Reference network architecture

The NetworkWalks reference architecture uses a host computer running VirtualBox to manage Kali Linux and optional target virtual machines.

| Machine | Reference IP address |
|---|---|
| Kali Linux | `10.0.0.2` |
| Target VM 1 | `10.0.0.7` |
| Target VM 2 | `10.0.0.9` |
| Target VM 3 | `10.0.0.10` |
| Target VM 4 | `10.0.0.11` |
| Target VM 5 | `10.0.0.16` |

These target addresses are taken from the reference diagram. They describe the proposed lab architecture and are not evidence that those machines are running or reachable.

The reference material also specifies a shared host directory at `/downloads` and internet connectivity for Kali Linux.

## 3. Methodology

### Phase 1: Virtualisation and lab preparation

#### Step 1: Install prerequisites

Install 7-Zip and VirtualBox on the host computer.

- [7-Zip Downloads](https://www.7-zip.org/download.html)
- [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

**Purpose:** 7-Zip supports archive extraction, while VirtualBox provides the platform for creating and managing the virtual machines used in the lab.

#### Step 2: Configure the virtual network

Create a custom NAT Network in VirtualBox using the following parameters:

- **Network type:** NAT Network
- **Subnet:** `10.0.0.0/24`
- **Reference address range:** `10.0.0.2–10.0.0.99`

Attach Kali Linux and any subsequent target virtual machines to this network.

**Purpose:** A common virtual network allows lab machines to communicate with each other while using VirtualBox NAT networking for external connectivity.

**Reference screenshot — creating the NAT Network:**

![VirtualBox NAT Network creation settings](<1. Creating the NatNetwork.png>)

> **Security consideration:** NAT networking is not equivalent to complete isolation. Verify the actual network configuration and avoid connecting vulnerable lab targets to production networks or exposing their services unnecessarily.

#### Step 3: Download and import Kali Linux

Download a Kali Linux virtual-machine image from the official source and import it into VirtualBox.

- [Kali Linux Downloads](https://www.kali.org/get-kali/)

Configure the VM's network adapter to use the custom NAT Network created in Step 2.

**Reference screenshot — selecting the correct network adapter:**

![VirtualBox network adapter configuration](<2. Ensuring it uses the right adapter.png>)

**Purpose:** Kali Linux serves as the primary machine for security testing and subsequent hands-on exercises.

#### Step 4: Configure Kali Linux networking

Configure the Kali Linux network interface with the static IPv4 address specified by the lab:

| Setting | Value |
|---|---|
| IP address | `10.0.0.2` |
| Subnet mask | `255.255.255.0` |
| CIDR notation | `10.0.0.2/24` |

Configure the appropriate gateway and DNS settings for the virtual network.

The NetworkWalks reference material suggests trying `10.0.0.1` if internet connectivity problems occur. Verify this against the actual VirtualBox NAT Network configuration rather than assuming it is the correct gateway in every environment.

**Purpose:** A predictable IP address simplifies connectivity testing and helps identify the attacking machine during later exercises.

**Reference screenshot — DNS server settings:**

![DNS server configuration](<3. Set DNS Servers .png>)

Use this image as a reference for the DNS settings shown in the lab. Confirm the values against your current network configuration before applying them.

#### Step 5: Enable virtual-machine integration features

Configure the Kali Linux VM to enable the integration features required by the lab:

- Shared clipboard.
- Drag-and-drop.
- Shared folders, with the host's `/downloads` directory made available to the VM.

**Purpose:** These features simplify file transfer and interaction between the host and guest virtual machine.

Only share directories that are necessary for the exercises. Avoid placing sensitive documents or credentials in a folder accessible to potentially compromised lab machines.

**Reference screenshot — shared folder configuration:**

![VirtualBox shared folder settings](<4. Shared Folder .png>)

#### Step 6: Create a virtual-machine snapshot

Create a snapshot of the Kali Linux VM after completing the initial configuration.

**Purpose:** A snapshot provides a recovery point that can be used to return the machine to its initial lab state after configuration changes or later exercises.

**Reference screenshot — VM snapshot:**

![Virtual machine snapshot](<5. Snapshot of VM.png>)

### Phase 2: Network connectivity troubleshooting

The NetworkWalks handout describes internet connectivity issues that may occur with certain VirtualBox 7 and Kali Linux 2026.1-or-newer combinations.

If the Kali VM cannot access the internet, first check:

1. Whether the custom NAT Network was created correctly.
2. Whether the VM's network adapter is attached to the intended network.
3. Whether the IP address conflicts with another VM using `10.0.0.2`.
4. Whether the gateway and DNS configuration are appropriate for the network.

The following commands are supplied by the NetworkWalks reference material as a possible workaround for the described connectivity issue.

First, modify the IPv4 duplicate-address-detection timeout for the named NetworkManager connection:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

Next, deactivate the connection so the updated configuration can be applied:

```bash
sudo nmcli connection down "Wired connection 1"
```

Reactivate the connection:

```bash
sudo nmcli connection up "Wired connection 1"
```

These commands assume the NetworkManager connection is named `Wired connection 1`. Check the actual connection name if the commands do not apply to the system.

The reference material recommends restarting Kali Linux if necessary. It also suggests restarting the virtual machines and host operating system if connectivity problems persist.

**Important:** These are troubleshooting steps from the lab handout, not a record of a confirmed fix in this environment. Their effectiveness depends on the underlying network configuration and the specific issue. Disabling or changing duplicate-address-detection behaviour should not replace checking for an actual IP address conflict.

### Phase 3: Optional target-machine preparation

The NetworkWalks exercise proposes installing additional virtual machines for future practical tasks.

Potential targets listed in the reference architecture include Windows 10, Windows 7, Windows Server 2016, and Android virtual machines.

**Reference screenshot — Android VM network configuration:**

![Android virtual machine network configuration](<6. Android Config.png>)

For each additional VM:

1. Attach the VM to the custom NAT Network.
2. Assign an appropriate, non-conflicting IP address within the lab subnet.
3. Verify connectivity between the machines.
4. Take a snapshot before beginning subsequent exercises.

**Purpose:** Multiple virtual machines provide a controlled environment for future network discovery, vulnerability assessment, and authorised security testing.

These are preparatory steps. No target enumeration, exploitation, or post-exploitation results are documented in this lab.

#### Known issue: Android and Kali connectivity

**Status: unresolved — follow-up task.** I was not able to successfully ping the Android virtual machine from Kali Linux, or establish the expected ping connectivity between the Android and Kali machines. The network configuration still needs to be investigated, so this README does not claim that inter-VM connectivity is working.

**Troubleshooting plan for a future session:**

1. Confirm both VMs are powered on and their virtual network adapters are connected to the same intended VirtualBox network.
2. Check each VM's current IP address and subnet mask, and verify that the addresses are unique and within the intended subnet.
3. Check the VirtualBox NAT Network settings and confirm the Android VM is attached to that NAT Network rather than a different adapter mode or network.
4. Test connectivity in both directions and record the exact error or timeout behaviour.
5. Check whether the Android environment or its firewall blocks ICMP echo requests; a failed ping alone does not prove that the VMs cannot communicate over other protocols.
6. Re-test after any single configuration change and record the result, rather than assuming the issue is fixed.

**Result to record later:** the Android and Kali IP addresses, their adapter/network settings, the exact ping commands and outputs, and any change that resolves the issue.

## 4. Key Findings and Configuration Considerations

This is an environment-setup lab, not a vulnerability-exploitation exercise. No specific target vulnerability was identified in the supplied lab notes.

The principal technical considerations are:

- **Network configuration:** Participating machines must use the intended virtual network and compatible subnet settings.
- **IP address conflicts:** Duplicate static IP addresses can cause connectivity problems.
- **Internet connectivity:** Incorrect NAT Network settings, gateway configuration, or DNS settings may prevent external access.
- **Virtual-machine integration:** Clipboard, drag-and-drop, and shared-folder settings affect usability and file movement between systems.
- **Recovery and repeatability:** Snapshots help preserve a known starting state for subsequent exercises.

The troubleshooting procedure highlights that a connectivity issue does not necessarily indicate a security vulnerability. It may instead result from a virtual-network or configuration problem.

## 5. Remediation and Defensive Considerations

Although this lab does not identify a specific vulnerability, several security practices should be applied when building and maintaining a cybersecurity testing environment.

### Network isolation

- Use a dedicated virtual network for security testing.
- Avoid exposing intentionally vulnerable target machines directly to the internet.
- Review VirtualBox network-adapter settings and disable unnecessary bridged or additional network interfaces.
- Verify that the configured network provides the intended level of separation.

### Secure file sharing

- Share only the directories required for the exercise.
- Avoid exposing sensitive host files to guest virtual machines.
- Disable clipboard sharing and drag-and-drop when they are no longer required.
- Review shared-folder permissions and access requirements.

### Virtual-machine maintenance

- Keep the host operating system, VirtualBox, and Kali Linux appropriately updated.
- Create snapshots before significant configuration changes or new exercises.
- Maintain a clean baseline image for repeatable testing.
- Use isolated, authorised targets for security assessments.

### Network troubleshooting

- Verify IP addresses, subnet masks, gateways, and DNS settings.
- Check for duplicate IP addresses before changing network parameters.
- Confirm that NetworkManager is managing the intended interface.
- Apply configuration changes deliberately and verify connectivity afterward.

## 6. Validation Checklist

Mark each item complete only after it has been configured and tested in the lab.

- [ ] VirtualBox installed.
- [ ] Custom NAT Network configured with subnet `10.0.0.0/24`.
- [ ] Kali Linux VM imported and configured.
- [ ] Kali Linux assigned `10.0.0.2/24`.
- [ ] Clipboard and drag-and-drop settings configured.
- [ ] Host `/downloads` directory shared with Kali Linux.
- [ ] Internet connectivity tested.
- [ ] Connectivity between Kali Linux and the Android VM verified. **Currently unresolved; follow-up required.**
- [ ] Initial Kali Linux snapshot created.

## 7. Key Takeaways

This lab establishes the foundational infrastructure needed for subsequent cybersecurity practical exercises.

Key learning areas include:

- Understanding virtualisation as a way to build repeatable testing environments.
- Configuring a custom NAT Network and static IPv4 addressing.
- Recognising the relationship between IP addressing, gateways, DNS, and connectivity.
- Using NetworkManager's `nmcli` utility to modify and reactivate network connections.
- Understanding the operational trade-offs associated with clipboard sharing and shared folders.
- Using snapshots to support recovery and repeatable lab work.
- Preparing a dedicated environment for authorised cybersecurity testing.

The configured environment is intended to provide a foundation for future hands-on exercises involving network reconnaissance, vulnerability assessment, and other security-testing techniques. The validation checklist above can be used to record which setup steps have actually been completed.

## 8. References

- [NetworkWalks](https://networkwalks.com/) — Training platform and lab instructions.
- [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads) — Virtualisation software.
- [Kali Linux Downloads](https://www.kali.org/get-kali/) — Kali Linux images and installation resources.
- [7-Zip Downloads](https://www.7-zip.org/download.html) — Archive utility.

---

**Disclaimer:** This repository documents cybersecurity learning activities performed in a lab environment. All scanning, testing, and exploitation exercises should be conducted only against systems for which explicit authorisation has been granted.
