# SDN-Based Access Control System

## Description
An SDN-based access control system using Mininet and POX controller.
The system maintains a whitelist of authorized hosts and blocks unauthorized access using OpenFlow rules.
Only authorized hosts (h1, h2) can communicate. Unauthorized hosts (h3) are blocked.

## Tools Used
- Mininet
- POX Controller
- Wireshark
- Python3


## Network Topology
The network consists of 3 hosts connected to a single switch,
which is controlled by the POX SDN Controller.

- h1 (10.0.0.1) → connected to Switch s1 (Port 1) → AUTHORIZED
- h2 (10.0.0.2) → connected to Switch s1 (Port 2) → AUTHORIZED  
- h3 (10.0.0.3) → connected to Switch s1 (Port 3) → UNAUTHORIZED
- Switch s1 → connected to POX Controller (c0)

All traffic passes through Switch s1.
POX Controller decides which hosts can communicate.

## How to Run
1. Start POX Controller:
   python3 pox.py log.level --DEBUG access_control

2. Start Mininet:
   sudo mn --controller=remote --topo single,3
   
## Testing

### Test 1 - Verify Access Control
   mininet> pingall

### Test 2 - Authorized Communication
   mininet> h1 ping -c 3 h2

### Test 3 - Unauthorized Access
   mininet> h3 ping -c 3 h1

### Test 4 - Regression Test
   mininet> h1 ping -c 3 h2  
   mininet> h2 ping -c 3 h1  
   mininet> h3 ping -c 3 h2  
   mininet> h3 ping -c 3 h1  
   
## Results
| Test | Expected | Result |
|------|----------|--------|
| h1 ping h2 | 0% loss | ✅ Pass |
| h2 ping h1 | 0% loss | ✅ Pass |
| h3 ping h1 | 100% loss | ✅ Pass |
| h3 ping h2 | 100% loss | ✅ Pass |

## How it Works
1. POX Controller starts and loads whitelist
2. Mininet creates network with h1, h2, h3 and switch s1
3. When a packet arrives at switch:
   - Controller checks source IP
   - If IP is in whitelist → packet is forwarded 
   - If IP is not in whitelist → packet is dropped 
4. ARP packets are always allowed for address resolution

## Key Concepts
| Concept | Explanation |
|---------|-------------|
| SDN | Software Defined Networking - separates control plane from data plane |
| Control Plane | POX Controller - decides where packets go |
| Data Plane | Switch s1 - forwards packets based on controller rules |
| Whitelist | List of authorized hosts allowed to communicate |
| OpenFlow | Protocol used between POX controller and switch |
| ARP | Address Resolution Protocol - maps IP to MAC address |

## References
1. Mininet - http://mininet.org
2. POX Controller - https://github.com/noxrepo/pox
3. OpenFlow - https://opennetworking.org
