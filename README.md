# SDN-Based Access Control System

## Description
An SDN-based access control system using Mininet and POX controller.
Only authorized hosts (h1, h2) can communicate. Unauthorized hosts (h3) are blocked.

## Tools Used
- Mininet
- POX Controller
- Wireshark
- Python3

## How to Run
1. Start POX Controller:
   python3 pox.py log.level --DEBUG access_control

2. Start Mininet:
   sudo mn --controller=remote --topo single,3

3. Test:
   pingall

## Results
- h1 and h2 can communicate (0% packet loss)
- h3 is blocked (100% packet loss)
