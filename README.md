# Innoviv Azure Route Tables (UDR) Routing Lab

## Overview

This project demonstrates how Azure User Defined Routes (UDRs) can be used to control network traffic flow within an Azure Virtual Network. The lab simulates a simple enterprise network where outbound traffic from an application subnet is redirected through a dedicated Linux firewall/router before accessing external networks.

This experiment provided hands-on experience with Azure networking, custom route tables, Linux packet forwarding, and network troubleshooting.

---

## Objectives

- Create an Azure Virtual Network with multiple subnets.
- Deploy a Linux firewall/router virtual machine.
- Deploy an application server in a separate subnet.
- Configure a User Defined Route (UDR).
- Force outbound traffic from the application subnet through the firewall VM.
- Configure Linux IP forwarding and NAT.
- Validate and troubleshoot network routing.

---

## Technologies Used

- Microsoft Azure
- Azure Virtual Network (VNet)
- Azure Route Tables
- User Defined Routes (UDR)
- Ubuntu Server
- Linux Networking
- IP Forwarding
- iptables (NAT)
- SSH

---

## Network Architecture

### Virtual Network

| Resource | Configuration |
|----------|---------------|
| Virtual Network | Innoviv-VNET |
| Address Space | 10.50.0.0/16 |

### Subnets

| Subnet | Address Range | Purpose |
|---------|---------------|---------|
| Innoviv-FW-Subnet | 10.50.10.0/24 | Firewall Router |
| Innoviv-App-Subnet | 10.50.20.0/24 | Application Server |
| Innoviv-DB-Subnet | 10.50.30.0/24 | Database Server |

---

## Virtual Machines

### Innoviv-FW01

Role:

Linux Firewall / Router

Private IP:

```
10.50.10.4
```

Configuration:

- IP Forwarding enabled
- Linux IPv4 forwarding enabled
- NAT configured using iptables
- Internet connectivity verified

---

### Innoviv-APP01

Role:

Application Server

Private IP:

```
10.50.20.4
```

---

## User Defined Route Configuration

Route Table

```
Innoviv-App-UDR
```

Configured Route

| Destination | Next Hop Type | Next Hop |
|-------------|---------------|----------|
| 0.0.0.0/0 | Virtual Appliance | 10.50.10.4 |

The route table was associated with the **Innoviv-App-Subnet**, forcing outbound traffic to the firewall VM.

---

## Firewall Configuration

IPv4 forwarding was enabled:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Network Address Translation (NAT) was configured using:

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Firewall internet connectivity was verified successfully.

---

## Validation

The following components were successfully validated:

- Azure Virtual Network deployment
- Subnet segmentation
- Route table creation
- User Defined Route configuration
- Route table association
- Linux IP forwarding
- NAT (MASQUERADE)
- Firewall VM internet connectivity

During testing, outbound traffic from the application server did not successfully traverse the firewall VM. The routing configuration and Linux forwarding were verified, and the remaining troubleshooting focused on Azure packet forwarding and traffic flow validation. This troubleshooting process provided valuable insight into Azure networking behavior and enterprise routing diagnostics.

---

## Repository Structure

```
innoviv-azure-udr-routing-lab
│
├── README.md
└── screenshots
    ├── 01-vnet-subnets.png
    ├── 02-fw01-vm.png
    ├── 03-app01-vm.png
    ├── 04-route-table.png
    ├── 05-effective-routes.png
    ├── 06-udr-route.png
    ├── 07-route-table-association.png
    ├── 08-masquerade-nat.png
    ├── 09-fw01-internet-connectivity.png
    └── 10-azure-export-template.png
```

---
## Screenshots & Evidence

### Azure Virtual Network and Subnets

The Innoviv network was segmented into firewall, application, and database subnets.

![Innoviv VNet Subnets](screenshots/01-vnet-subnets.png)


### Firewall Router VM

Innoviv-FW01 was deployed as the Linux firewall/router appliance.

![Innoviv Firewall VM](screenshots/02-fw01-vm.png)


### Application VM

Innoviv-APP01 was deployed in the application subnet.

![Innoviv Application VM](screenshots/03-app01-vm.png)


### Route Table Configuration

A custom Azure Route Table was created to control application traffic.

![Route Table](screenshots/04-route-table.png)


### Effective Routes

The application subnet received the custom route:

```
0.0.0.0/0 → Virtual Appliance → 10.50.10.4
```

![Effective Routes](screenshots/05-effective-routes.png)


### User Defined Route (UDR)

Traffic was configured to flow through the firewall virtual appliance.

![UDR Route](screenshots/05-udr-route.png)


### Route Table Association

The route table was associated with the Innoviv application subnet.

![Route Association](screenshots/06-route-table-association.png)


### NAT Configuration

Linux NAT was configured using iptables MASQUERADE.

![MASQUERADE Configuration](screenshots/07-masquerade-nat.png)


### Firewall Internet Connectivity

The firewall VM successfully reached the internet.

![Firewall Ping Test](screenshots/08-fw01-internet-connectivity.png)


### Azure Deployment Template

Azure resource configuration export.

![Azure Export Template](screenshots/09-azure-export-template.png)