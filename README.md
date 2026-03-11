# Networking Automation using Claude AI, MCP Server, and SONiC

Automate network management for spine-and-leaf topologies running SONiC with VXLAN EVPN configuration. This project leverages Claude AI and the Model Context Protocol (MCP) to provide intelligent, conversational network automation through GNS3.

## 🚀 Overview

This project combines **GNS3** with **Broadcom SONiC routers**, **Claude AI**, and the **Model Context Protocol (MCP)** to deliver intelligent network automation. Query your network topology, execute show commands, and push configurations through natural language—all powered by Claude.

### Key Features

- **AI-Powered Automation**: Use Claude AI to intelligently query and manage network configurations
- **MCP Server Integration**: Seamless integration with Claude through MCP for real-time network operations
- **Spine-Leaf Topology**: Pre-configured multi-router architecture with automated management
- **BGP Underlay Configuration**: BGP-based routing for scalable network design
- **VXLAN EVPN Overlay**: Automated overlay networking with Layer 2 and Layer 3 VNI support
- **SSH-Based Device Management**: Direct connectivity to all network devices via Netmiko
- **Alpine Linux Endpoints**: Multi-VLAN support with lightweight server endpoints

## 📋 Prerequisites

- **GNS3** with Broadcom SONiC router images
- **Python 3.8** or higher
- **Claude API** access with MCP Server capability
- **Broadcom SONiC router images** for GNS3 deployment
- **SSH access** configured for network devices
- **Alpine Linux images** for endpoint deployment in GNS3

## ⚒️ Project Tech Stack

- Claude AI (via Code Interpreter)
- MCP Server (FastMCP)
- GNS3 (Network Simulator)
- Python 3.x
- Netmiko (Network Device Connection)
- Broadcom SONiC OS
- Alpine Linux
- VS Code

## 🛠️ Project Steps

### 1️⃣ Inventory Configuration

Create a JSON inventory file containing your spine and leaf routers with their respective types.

**Note:** Netmiko recognizes two SONiC types:
- **Dell SONiC**: Works identically for Broadcom SONiC; connects directly to the CLI interface
- **Edge SONiC**: Uses Community SONiC; connects to the Linux interface

```json
{
  "Spine": { "host": "192.168.108.20", "device_type": "dell_sonic" },
  "Leaf1": { "host": "192.168.108.30", "device_type": "dell_sonic" },
  "Leaf2": { "host": "192.168.108.10", "device_type": "dell_sonic" }
}
```

### 2️⃣ Network Topology

The topology consists of:
- **1 Broadcom SONiC Spine Router**
- **2 Broadcom SONiC Leaf Routers**
- **4 Alpine Linux Servers** (2 per leaf router, in separate VLANs)

![Network Topology](topology.png)

### 3️⃣ SONiC Configuration

Configure spine and leaf routers with VXLAN EVPN and BGP underlay:
- **VLAN 10 & 11**: Layer 2 VNI (L2 overlay)
- **VLAN 200**: Layer 3 VNI (L3 overlay)
- **BGP**: Underlay routing protocol

Full configuration details are available in the `sonic_vxlan.txt` file.

### 4️⃣ Connect MCP Server to Claude

#### Step 1: Register the MCP Server

```bash
claude mcp add mcpsonic_automation -s user -- "<mcp_server_location>" "<path_to_python_script>"
```

#### Step 2: Configure Your MCP Python Script

The `mcpserversonic.py` script connects Claude AI to your GNS3 topology. It includes:

- **FastMCP Integration**: For seamless MCP protocol communication
- **`run_show()` Function**: Executes show commands to query network state
- **`push_config()` Function**: Pushes configurations to devices

The script uses **Netmiko** with SSH to connect to all routers. SONiC routers have SSH enabled by default.

```python
# Example structure:
from fastmcp import FastMCP

mcp = FastMCP("mcpsonic_automation")

@mcp.tool()
async def run_show(device: str, command: str) -> str:
    """Execute show commands on network devices"""
    # Implementation using Netmiko

@mcp.tool()
async def push_config(device: str, commands: list) -> str:
    """Push configurations to network devices"""
    # Implementation using Netmiko
```

### 5️⃣ Test the MCP Server

#### Example Query 1: Check VXLAN Tunnel Status

Send this command to Claude:
```
Can you check the VXLAN tunnel between Leaf1 and Leaf2 and the VRF configurations?
```

Claude will:
- Connect to your topology via the MCP server
- Execute `show vxlan tunnel` and related commands
- Return detailed results

![VXLAN Query Result](https://github.com/user-attachments/assets/c90daa6c-4b0e-457a-b59a-93b7bfa15b0e)

#### Example Query 2: Update Device Configuration

Send this command to Claude:
```
Add description "future use" to interface Ethernet40 on Leaf2
```

Claude will:
- Use the `push_config()` function
- Connect to Leaf2
- Apply the configuration change
- Confirm completion

![Configuration Update Result](https://github.com/user-attachments/assets/14cabc7f-bf29-4ab7-90aa-1a7991ebc2b9)

## 🚀 Getting Started

1. **Deploy GNS3 topology** with SONiC images
2. **Create inventory** JSON file with your router details
3. **Configure SONiC devices** with VXLAN EVPN and BGP
4. **Set up MCP server** with the provided Python script
5. **Register with Claude** using the MCP add command
6. **Start automating** via natural language queries!

## 📝 License

[Add your license information here]

## 🤝 Contributing

Contributions are welcome! Please submit issues and pull requests.