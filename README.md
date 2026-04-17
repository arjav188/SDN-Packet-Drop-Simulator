The goal of this project is to simulate packet loss in a network using Software Defined Networking (SDN).

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

# Start POX Controller
cd ~/pox
./pox.py openflow.of_01 drop_controller


<img width="646" height="437" alt="Screenshot from 2026-04-17 06-34-27" src="https://github.com/user-attachments/assets/6a29205a-7526-4e8d-977d-bd0c1e0be1b3" />


<img width="641" height="284" alt="2" src="https://github.com/user-attachments/assets/b511faea-7f43-4354-b8a6-f2a1f87ca4f1" />

2. Start Mininet
sudo mn --topo single,2 --controller=remote,ip=127.0.0.1,port=6633

Test Scenarios
Scenario 1: Normal Network
sudo mn --topo single,2
pingall

Result: 0% packet loss


<img width="515" height="271" alt="normal" src="https://github.com/user-attachments/assets/b6b9f4ac-332c-4955-b3ca-c295fa542c13" />


Scenario 2: Packet Drop
pingall

Result: 100% packet loss


<img width="645" height="385" alt="3" src="https://github.com/user-attachments/assets/34a03c7e-6c94-4e60-8a32-c70d239f80f7" />


h1 ping h2

Performance Observation

- Latency: Ping fails when ICMP packets are dropped
- Throughput: Only ICMP traffic is affected; other protocols can be tested separately
- Flow Table: Drop rule appears dynamically after controller installation

- 
Flow Table Verification:
sudo ovs-ofctl dump-flows s1
<img width="759" height="28" alt="Screenshot from 2026-04-17 06-43-59" src="https://github.com/user-attachments/assets/ab4a4f84-79de-438d-b36f-59f33a188bd2" />
The flow table shows a rule matching ICMP traffic (nw_proto=1) with action drop, confirming packet filtering.

Validation / Regression Testing
The experiment was repeated multiple times
Packet drop behavior remained consistent
Flow rules were correctly reinstalled on each run

Conclusion

This project demonstrates how SDN enables centralized control of network behavior. By installing flow rules, specific traffic (ICMP) can be selectively dropped, allowing simulation of packet loss and network failure conditions.
