# Azure-Multi-VNet-Peering-Custom-Routing-Lab

## 📖 Project Overview
This project demonstrates the implementation of a robust Hub-and-Spoke network topology in Azure. I transitioned a manual Portal-based deployment into a fully parameterised Infrastructure as Code (IaC) solution. The lab focuses on interconnecting isolated environments (Core Services and Manufacturing) and controlling traffic flow via User-Defined Routes (UDRs).

## 🏗️ Architecture
The environment consists of two Virtual Networks located in UK South, interconnected via a low-latency peering link
***VNet 1:** CoreServicesVnet (10.0.0.0/16)

***Core Subnet:** Hosts the primary service VM (CoreServicesVM)

***Perimeter Subnet:** Reserved for NVA (Network Virtual Appliance) or gateway traffic

***VNet 2:** ManufactoringVnet (172.16.0.0/16)

***Manufacturing Subnet:** Hosts production-specific workloads (ManufactoringVM)

## 🛣️Routing Logic: User-Defined Routes (UDR)
To simulate security inspection and traffic redirection:
***Route Table (rt-CoreServices):** Applied to the Core Subnet

***Custom Hop:** Configured a route for the 10.0.0.0/16 range to pass through a Virtual Appliance at 10.0.1.7

***BGP Propagation:** Disabled to ensure the custom static routes maintain priority

## 🛠️ Infrastructure as Code (ARM)
The deployment is automated using a parameterised ARM template. Key improvements made to the template include:

***Location Portability:** Defined via a location parameter (defaults to uksouth)

***Dynamic Disk Naming:** Uses uniqueString to prevent naming collisions for OS Disks during redeployments.

***Security:** Passwords and sensitive data are moved to parameters for secure handling. Please note there is a placeholder password in parameters.json file. Ensure to change this.

## ✅ Validation & Troubleshooting
Connectivity was verified using industry-standard Azure tools:

***Network Watcher:** Used the Troubleshoot tool to verify the health of the Network Interfaces (NICs) and ensure NSGs weren't dropping valid traffic.

***PowerShell RunCommand:** Executed Test-NetConnection directly from the Azure Portal onto the VMs.

***Result:** Confirmed successful connectivity between 10.0.0.4 and 172.16.0.4 over the peered backbone.
