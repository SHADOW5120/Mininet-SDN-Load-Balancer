<img width="414" height="303" alt="image" src="https://github.com/user-attachments/assets/27250b0f-d4af-469b-8b24-46a7449064a9" /># SDN Load Balancer using Mininet and Floodlight

An implementation of a **Software-Defined Networking (SDN) Load Balancer** using Mininet, Open vSwitch (OVS), and a Floodlight-compatible REST API.

This project explores dynamic traffic distribution and network path optimization through a custom Fat-Tree topology. It combines shortest-path routing with link-cost evaluation and Least Connection load balancing to improve network utilization and reduce congestion.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Network Topology](#network-topology)
- [Project Structure](#project-structure)
- [Technologies](#technologies)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Load Balancing Algorithms](#load-balancing-algorithms)
- [Testing and Evaluation](#testing-and-evaluation)
- [Results](#results)
- [Troubleshooting](#troubleshooting)

---

## Overview

Traditional networks often rely on static routing configurations, which can lead to uneven traffic distribution and network congestion.

Software-Defined Networking (SDN) addresses this limitation by separating the control plane from the data plane, allowing centralized network management through an SDN Controller.

This project implements an SDN-based load balancing system with three main components:

1. **Fat-Tree Topology Construction:** Builds a virtual network consisting of 8 hosts and 10 Open vSwitch switches using Mininet.
2. **Shortest Path Calculation:** Collects network topology and link statistics through REST APIs, then uses Dijkstra's algorithm to identify shortest paths with the lowest transmission cost.
3. **Least Connection Load Balancing:** Monitors active flows and dynamically distributes incoming traffic to the server with the fewest active connections.

The objective is to improve traffic distribution, utilize available network paths more efficiently, and reduce network congestion.

<img width="538" height="400" alt="image" src="https://github.com/user-attachments/assets/27aea504-fec1-47ba-9d50-c5f450155e42" />
images/sdn_architecture.png
<!-- IMAGE: images/sdn_architecture.png -->
<!-- Add a system architecture diagram showing Mininet, Open vSwitch, Floodlight Controller, and the Python applications. -->

## Features

### 1. Custom Fat-Tree Topology

- Create a virtual Fat-Tree network using Mininet.
- Deploy 8 hosts and 10 Open vSwitch switches.
- Establish connections between hosts through multiple network paths.
- Support network connectivity testing using Ping.

### 2. Dynamic Network Monitoring

- Communicate with the SDN Controller through REST APIs.
- Retrieve connected devices and their MAC/IP addresses.
- Discover switch-to-switch links and port mappings.
- Collect network statistics, including TX and RX information.
- Monitor network conditions for routing decisions.

### 3. Shortest Path Routing

- Construct a network graph using NetworkX.
- Apply Dijkstra's algorithm to identify shortest paths.
- Evaluate link transmission costs.
- Select the path with the lowest calculated cost among the shortest paths.
- Install static flow entries into switches along the selected path.

### 4. Least Connection Load Balancing

- Monitor active flows directed to backend servers.
- Count the number of active flows associated with each server.
- Select the server with the fewest active flows.
- Dynamically install forwarding rules through the controller.

### 5. Network Performance Evaluation

- Measure bandwidth and data transfer using iPerf.
- Measure network latency using Ping.
- Inspect packet forwarding paths using Wireshark.
- Compare network performance before and after optimization.

---

## System Architecture

The system consists of three main layers:

| Component | Responsibility |
|---|---|
| Application Layer | Python scripts for routing calculation and load balancing. |
| Control Layer | Floodlight-compatible SDN Controller for network monitoring and flow management. |
| Infrastructure Layer | Mininet, Open vSwitch switches, and virtual hosts. |

The Python applications communicate with the SDN Controller through REST APIs to retrieve network information and install flow rules.

<img width="373" height="340" alt="image" src="https://github.com/user-attachments/assets/16aa6087-17e7-444e-9504-d116f5b07331" />
images/system_architecture.png
<!-- IMAGE: images/system_architecture.png -->
<!-- Add an architecture diagram illustrating communication between Python applications, Floodlight REST API, Open vSwitch, and Mininet hosts. -->

### Traffic Processing Workflow

1. Hosts generate network traffic.
2. Open vSwitch forwards packets according to installed flow rules.
3. The SDN Controller collects topology and network statistics.
4. Python applications retrieve network information through REST APIs.
5. The routing application calculates optimal paths.
6. The load balancer evaluates active flows and selects an appropriate backend server.
7. The controller installs forwarding rules into the relevant switches.

---

## Network Topology

The project uses a custom Fat-Tree topology implemented with Mininet.

### Network Components

| Component | Configuration |
|---|---|
| Virtual Hosts | 8 (h1 - h8) |
| Open vSwitch Switches | 10 |
| Host IP Range | 10.0.0.1 - 10.0.0.8 |
| Network Emulator | Mininet |
| Switch Technology | Open vSwitch |
| Controller | Floodlight-compatible SDN Controller |

### Switch Configuration

The topology contains the following switches:

```text
s1, s2, s3, s4, s10,
s11, s17, s18, s21, s22
```
<img width="590" height="300" alt="image" src="https://github.com/user-attachments/assets/ed363662-ad7b-490a-bb4c-b3dd55863c7d" />
images/fattree_topology.png
<!-- IMAGE: images/fattree_topology.png -->
<!-- Add a screenshot or diagram of the complete Fat-Tree topology, including all 8 hosts and 10 switches. -->

---

## Project Structure

```text
SDN-Load-Balancer/
│
├── fattree_topo.py
│   └── Mininet Fat-Tree topology definition.
│
├── route_calculator.py
│   └── Network discovery, shortest path calculation,
│       link-cost evaluation, and static flow installation.
│
├── least_connection_lb.py
│   └── Least Connection load balancing implementation.
│
├── images/
│   ├── sdn_architecture.png
│   ├── system_architecture.png
│   ├── fattree_topology.png
│   ├── controller_running.png
│   ├── mininet_topology.png
│   ├── pingall_test.png
│   ├── routing_result.png
│   ├── load_balancing_result.png
│   ├── iperf_test.png
│   ├── ping_test.png
│   └── wireshark_capture.png
│
└── README.md
```

---

## Technologies

| Technology | Purpose |
|---|---|
| Python 3 | Routing and load balancing applications |
| Mininet | Virtual network emulation |
| Open vSwitch (OVS) | Software-based network switches |
| Floodlight | SDN Controller and REST API |
| NetworkX | Graph construction and shortest path algorithms |
| REST API | Controller communication and flow management |
| iPerf | Bandwidth and throughput testing |
| Wireshark | Packet capture and network traffic analysis |
| cURL | REST API testing and HTTP requests |

---

## Prerequisites

The project is designed to run in an Ubuntu environment.

### Required Software

| Software | Version / Configuration |
|---|---|
| Ubuntu | 22.04 LTS or 24.04 LTS |
| Python | Python 3 |
| Mininet | Compatible Mininet installation |
| Floodlight | Version 1.2 (if using Floodlight) |
| Open vSwitch | Compatible with Mininet |
| JDK | JDK 8 (for Floodlight) |
| Git | Latest available version |
| Apache Ant | For building Floodlight if required |
| iPerf | For network performance testing |
| Wireshark | For packet inspection |
| cURL | For REST API communication |

### Python Dependencies

Install the required Python libraries:

```bash
pip install requests networkx
```

### Install Mininet

```bash
sudo apt update
sudo apt install mininet openvswitch-switch -y
```

Verify the installation:

```bash
sudo mn --version
```

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/SHADOW5120/Mininet-SDN-Load-Balancer
cd SDN-Load-Balancer
```

### 2. Install Python Dependencies

```bash
pip install requests networkx
```

### 3. Start the SDN Controller

Navigate to the Floodlight directory and start the controller:

```bash
java -jar target/floodlight.jar
```

The controller should expose its REST API on port `8080` and communicate with OpenFlow switches through port `6653`, according to the controller configuration.

<img width="424" height="303" alt="image" src="https://github.com/user-attachments/assets/75b7eade-1c58-41a2-a778-9e6a1b39b64e" />
images/controller_running.png
<!-- IMAGE: images/controller_running.png -->
<!-- Add a screenshot showing Floodlight successfully starting in the terminal. -->

### 4. Start the Mininet Topology

Open another terminal and navigate to the project directory.

Run:

```bash
sudo mn --custom fattree_topo.py --topo mytopo --controller=remote,ip=127.0.0.1,port=6653 --mac
```

This command creates the virtual network and connects Mininet to the remote SDN Controller.

<img width="414" height="303" alt="image" src="https://github.com/user-attachments/assets/8bf4d4ab-0642-48b3-85c2-1d9ea642a249" />
images/mininet_topology.png
<!-- IMAGE: images/mininet_topology.png -->
<!-- Add a screenshot showing the Mininet topology successfully initialized. -->

### 5. Verify Network Connectivity

Inside the Mininet CLI, execute:

```bash
pingall
```

This command checks connectivity between all virtual hosts.

Expected result:

```text
*** Results: 0% dropped
```

<img width="410" height="303" alt="image" src="https://github.com/user-attachments/assets/6d63150e-34c9-4c92-8b4d-2ac4870f04e9" />
images/pingall_test.png
<!-- IMAGE: images/pingall_test.png -->
<!-- Add a screenshot showing successful pingall results. -->

<img width="416" height="203" alt="image" src="https://github.com/user-attachments/assets/3210b8fa-d045-4137-816b-988da2ebd39f" />
images/h4_h1_trafic_bandwith.png

<img width="416" height="213" alt="image" src="https://github.com/user-attachments/assets/f468ed4e-ae93-47fc-b342-76e3e10f9610" />
images/h4_h1_latency.png

---

## Usage

### 1. Shortest Path Calculation

The routing application retrieves topology and link statistics from the SDN Controller.

Main operations:

1. Retrieve connected devices and host information.
2. Discover switch links and port mappings.
3. Collect network transmission statistics.
4. Construct a graph representing the network.
5. Apply Dijkstra's algorithm to identify shortest paths.
6. Evaluate the transmission cost of candidate paths.
7. Install static forwarding rules along the selected path.

### 2. Least Connection Load Balancing

Run:

```bash
python3 loadbalancer.py
```

The application continuously monitors active flows directed to backend servers.

When a new host requests a connection, the application:

1. Retrieves current flow information.
2. Counts active flows associated with each backend server.
3. Identifies the server with the lowest number of active flows.
4. Installs forwarding rules through the SDN Controller.
5. Continues monitoring network activity.

<img width="416" height="213" alt="image" src="https://github.com/user-attachments/assets/cd7e4368-3276-49d9-aaf5-a5c64ba6d8c2" />
<img width="443" height="301" alt="image" src="https://github.com/user-attachments/assets/53811368-0351-4524-b499-5d2e02482d4d" />
<img width="464" height="315" alt="image" src="https://github.com/user-attachments/assets/c710a454-5b33-469b-a917-4aa02f8d86d7" />
<!-- IMAGE: images/load_balancing_result.png -->
<!-- Add a screenshot showing server selection and active flow counts. -->

---

## Load Balancing Algorithms

### Dijkstra's Shortest Path Algorithm

Dijkstra's algorithm is used to determine the shortest paths between network nodes.

In this project, the algorithm is combined with link-cost evaluation to select a path with a lower transmission cost among the available shortest paths.

The routing process can be summarized as follows:

```text
Network Topology
       |
       v
Collect Network Statistics
       |
       v
Construct Network Graph
       |
       v
Find Shortest Paths
       |
       v
Calculate Link Costs
       |
       v
Select Lowest-Cost Path
       |
       v
Install Static Flow Rules
```

### Least Connection Algorithm

The Least Connection algorithm distributes incoming traffic according to the number of active flows associated with each backend server.

The basic selection rule is:

```text
Selected Server = Server with Minimum Active Connections
```

This approach aims to prevent traffic from being concentrated on a single server.

---

## Testing and Evaluation

The project evaluates network performance before and after applying routing optimization and load balancing.

### 1. Bandwidth and Throughput Testing

Use iPerf to measure data transfer performance.

Example:

Start the server on h1:

```bash
h1 iperf -s
```

Run the client on h4:

```bash
h4 iperf -c 10.0.0.1
```

Record the following metrics:

- Data transferred.
- Bandwidth.
- Transmission performance.

<!-- IMAGE: images/iperf_test.png -->
<!-- Add screenshots of iPerf results before and after optimization. -->

### 2. Network Latency Testing

Use Ping to measure the round-trip time between hosts.

```bash
h4 ping -c 10 10.0.0.1
```

Record:

- Minimum latency.
- Average latency.
- Maximum latency.
- Standard deviation (mdev).

<!-- IMAGE: images/ping_test.png -->
<!-- Add screenshots of Ping results before and after optimization. -->

### 3. Packet Forwarding Analysis with Wireshark

Use Wireshark to inspect traffic passing through selected switch ports.

The objective is to verify whether traffic is forwarded through the paths selected by the routing application.

The analysis focuses on:

- Source and destination IP addresses.
- Packet forwarding paths.
- Switch interfaces and port numbers.
- Traffic distribution across network links.

<img width="492" height="169" alt="image" src="https://github.com/user-attachments/assets/f888457e-142c-4229-9cfe-807f239c8c16" />
images/h4_package_before_LB.png

<img width="492" height="196" alt="image" src="https://github.com/user-attachments/assets/1bc0f81f-5d27-44ea-a000-9e57259a272d" />
images/h3_package_before_LB.png

<img width="532" height="323" alt="image" src="https://github.com/user-attachments/assets/102dc1ea-6738-4c5f-a4c1-6117bc0b704e" />
images/h4_package_after_LB.png

<img width="551" height="330" alt="image" src="https://github.com/user-attachments/assets/87ef3eeb-d543-48ae-9828-32b514d2066d" />
images/h3_package_after_LB.png

<img width="556" height="263" alt="image" src="https://github.com/user-attachments/assets/ff3e21e1-25a9-4a82-bc7a-3bbda95538d9" />
<img width="548" height="188" alt="image" src="https://github.com/user-attachments/assets/6f568e83-8b84-4de6-9365-4943e0bebf4e" />
images/h3_h4_route_to_port4_switch1.png

<!-- IMAGE: images/wireshark_capture.png -->
<!-- Add Wireshark screenshots demonstrating packet forwarding before and after optimization. -->
Observation: Packets from h3 have been forwarded through port 4 of switch 1, avoiding congestion at port 3.

---

## Results

The following metrics can be used to compare network performance before and after optimization:

| Metric | Before Optimization | After Optimization |
|---|---|---|
| Average Latency | To be measured | To be measured |
| Average Bandwidth | To be measured | To be measured |
| Data Transfer | To be measured | To be measured |
| Traffic Distribution | To be analyzed | To be analyzed |

<!-- IMAGE: images/performance_comparison.png -->
<!-- Add a chart comparing average latency and bandwidth before and after optimization. -->

The expected outcome is improved traffic distribution, reduced congestion on heavily utilized links, and better network performance.

Actual performance improvements depend on network topology, traffic patterns, link conditions, and controller configuration.

---

## Troubleshooting

### 1. Mininet Cannot Connect to the Controller

Check whether Floodlight is running:

```bash
curl http://127.0.0.1:8080/wm/core/controller/switches/json
```

Verify that the OpenFlow port is configured correctly:

```text
6653
```

### 2. Network Connectivity Issues

Inside Mininet:

```bash
pingall
```

Check switch connections:

```bash
net
```

Check Open vSwitch status:

```bash
sudo ovs-vsctl show
```

### 3. REST API Connection Errors

Verify that the controller REST API is accessible:

```bash
curl http://127.0.0.1:8080/wm/core/controller/switches/json
```

Check the controller configuration and ensure that the required REST API modules are enabled.

### 4. Missing Python Dependencies

Install the required libraries:

```bash
pip install requests networkx
```

---

## Future Improvements

Potential improvements for this project include:

- Integrating additional load balancing algorithms such as Weighted Round Robin.
- Implementing more advanced link-cost models.
- Incorporating real-time congestion detection.
- Adding server health monitoring.
- Improving the handling of network topology changes.
- Developing a dashboard for real-time network visualization.
- Evaluating performance under different network traffic patterns.

---

## License

This project is developed for educational and research purposes.

---

## Author

Developed as an academic project focusing on **Software-Defined Networking, Network Optimization, and Dynamic Load Balancing**.
