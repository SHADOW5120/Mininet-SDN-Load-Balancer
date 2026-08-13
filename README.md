## Overview

This project implements a Software-Defined Networking (SDN) load balancer using Mininet. It creates a custom **Fat-Tree topology** and utilizes REST APIs to interact with an SDN Controller (likely Floodlight, given the REST endpoints used). 

The project includes scripts to:
1.  **Define the Topology:** Create a network of switches and hosts.
2.  **Calculate Shortest Paths & Link Costs:** Route traffic optimally based on dynamic bandwidth statistics (Link TX costs).
3.  **Least Connection Load Balancing:** Distribute traffic to servers based on the number of active flows (connections) to prevent overloading any single node.

## Repository Structure

*   `fattree_topo.py`: Mininet Python script to construct the Fat-Tree network topology consisting of 8 hosts and 10 Open vSwitch (OVS) nodes.
*   `route_calculator.py`: Script that queries the SDN controller's REST API to gather device MAC/IPs, switch links, and bandwidth statistics. It calculates the shortest path and pushes static flows to direct traffic based on minimal transmission cost.
*   `least_connection_lb.py`: The Load Balancer script. It continuously monitors the SDN controller to count active flows directed to specific servers. When a new host requests a connection, it dynamically pushes flow rules to route the traffic to the server currently handling the fewest connections.

## Prerequisites

*   **Mininet:** Network emulator.
*   **SDN Controller:** An SDN controller running locally on port 8080 (e.g., Floodlight) that supports the Static Flow Pusher and Topology/Device REST APIs.
*   **Python 3:** With `requests` and `networkx` modules installed.
    ```bash
    pip install requests networkx
    ```
*   **cURL:** Used by the Python scripts to execute system commands for pushing flows (though `requests.post` is also used in some places).

## Network Topology (Fat-Tree)

The topology consists of:
*   **8 Hosts:** h1 through h8 (IPs: `10.0.0.1` - `10.0.0.8`).
*   **10 Switches:** s1, s2, s3, s4, s10, s11, s17, s18, s21, s22.

*To start the topology in Mininet (assuming the script is named `fattree_topo.py`):*
```bash
sudo mn --custom fattree_topo.py --topo mytopo --controller=remote,ip=127.0.0.1,port=6653 --mac
