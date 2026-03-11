# Networking-automation-using-cluade-ai-MCP-server-and-Sonic
Networking automation using MCP server and Sonic. The topology is made of spine and leaf routers running sonic with vxlan evpn configuration. After bringing up the topology. Run broadband sonic on all the routers and configure the spine leaf with vxlan. Connect mcp server using claude ai, run show commands and push configurations to configure and manage the topology.

## 🚀 Overview

This project leverages Gns3 lab with three Broadcom sonic routers  Claude AI and the Model Context Protocol (MCP) to provide intelligent network automation capabilities. It combines the power of AI-driven decision making with industry-standard Ansible playbooks to manage Broadcom network devices efficiently.

### Key Features

- **AI-Powered Automation**: Use Claude AI to intelligently manage network configurations
- **MCP Server Integration**: Seamless integration with Claude through MCP for dynamic network operations
- **Ansible Integration**: Industry-standard playbooks for reliable network automation
- **Broadcom Gns3 Router Support**: Direct support for Broadcom Router devices
- ***BGP underlay configuration** : BGP is used as underlay configuration
- **Vxlan Evpn Configuration**: Automated OSPF routing protocol management
- **Alpine Linux as server endpoints**: Alpine servers in different vlans

  
## 📋 Prerequisites

- [x] Gns3 with Broadcom sonic image
- [x] Python 3.8 or higher
- [x] Access to Claude API with MCP Server capability
- [x] Broadcom sonic router images that will be deployed in Gns3
- [x] SSH access configured for your network devices
- [x] Alpine Linux images that will be deployed as endpoints on Gns3

## ⚒️ Project Tech Stack
The main tools and technologies used for building the project:
- [x] Claude AI (Claude Code)
- [x] MCP Server (FastMCP)
- [x] GNS3
- [x] Python
- [x] Netmiko (Sonic devices don't support Scrapli yet)
- [x] Broadcom 
- [x] Alpine Linux
- [x] VS Code

## Project Steps
- [x] The topology 

--- 
Consists of one Broadcom Spine connected to two Broadcom Leafs with two Alpine Linux connected to the each Leaf

---
![topology](sonic vlxan with mcp.png.png)

- [x] Configuration
---
Spine/ Leaf vxlan evpn configuration with BGP underlay. Vlan 10 and 11 are L2 VNI with vlan 200 is L3 VNI. The configuration file can befound at sonic_vxlan.conf file

---


- [x]  Create the newtwork json inventory file containing the spine and leaf routers and their types. Netmiko reconiges two types of Sonic Dell sonic which works the same for Broadcom and Edgesonic that uses Community Sonic. If you choose Dell sonic connection goes straight to Dell or Broadcom CLI interface. If you choose Edgesonic, Netmiko thats connections to the linux interface
```
{
	
  "Spine": { "host": "192.168.108.20", "device_type": "dell_sonic" },
  "Leaf1": { "host": "192.168.108.30", "device_type": "dell_sonic" },
  "Leaf2": { "host": "192.168.108.10", "device_type": "dell_sonic"}

}
```

      
