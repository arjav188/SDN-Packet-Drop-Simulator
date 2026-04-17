Problem Statement
The goal of this project is to simulate packet loss in a network using Software Defined Networking (SDN). This is achieved by installing OpenFlow rules that selectively drop packets (ICMP) in a Mininet-based network.

Objective
Understand SDN architecture (control plane vs data plane)
Implement match-action flow rules
Simulate packet loss using OpenFlow
Analyze network behavior under packet drop conditions

Network Topology
Single switch (s1)
Two hosts (h1, h2)
h1 ---- s1 ---- h2

This simple topology is used to clearly observe the effect of flow rules.

Tools & Technologies
Mininet
POX Controller
OpenFlow Protocol
Ubuntu (WSL)


Implementation Details
Flow Rule Logic:
Match:
dl_type = 0x0800 (IP packets)
nw_proto = 1 (ICMP)
Action:
No action → packet is dropped

This rule is installed when the switch connects to the controller.
Execution Steps
1. Start POX Controller
cd ~/pox
./pox.py openflow.of_01 drop_controller
<img width="641" height="231" alt="image" src="https://github.com/user-attachments/assets/57fbee03-76a9-44d1-86a9-0e0fb9e3abbd" />

2. Start Mininet
sudo mn --topo single,2 --controller=remote,ip=127.0.0.1,port=6633

Test Scenarios
✅ Scenario 1: Normal Network
sudo mn --topo single,2
pingall

Result: 0% packet loss

❌ Scenario 2: Packet Drop
pingall

Result: 100% packet loss

h1 ping h2
