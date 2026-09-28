# vlan-nat
Cisco Packet Tracer lab: VLAN segmentation, 802.1Q trunking, router-on-a-stick inter-VLAN routing, DHCP, and NAT overload with a simulated ISP.


This project simulates a small business network with three internal departments (Sales, IT, and Servers), each isolated on its own VLAN, routed by a single router using sub-interfaces ("router-on-a-stick"), with automatic IP addressing via DHCP and internet access via NAT overload (PAT) through a simulated ISP.




                                                          +----------------------+
                                                          |      Router1 (R1)    |
                                                          |  NAT + DHCP + Router |
                                                          |      on-a-stick      |
                                                          +-----------+----------+
                                                                      |
                                                          Gig0/0 (WAN)|  Gig0/2 (Trunk to Switch0)
                                                                      |
                              +------------------+                   |
                              |    Router0       |<------------------+
                              | (Simulated ISP)   |
                              +--------+---------+
                                       |
                                       | Gig0/1
                                       |
                              +--------+---------+
                              | Server1 (WebServer)|
                              |   8.8.8.10 /24     |
                              |   (static)          |
                              +--------------------+

                                       Gig0/2 <-> Fa0/1 (trunk)
                                                |
                                          +-----+------+
                                          |  Switch0   |
                                          | (2960-24TT)|
                                          +--+---+---+-+
                                             |   |   |
                                (access) Fa0/2-3 |   Fa0/6 (access)
                                          |      |Fa0/4-5
                          +---------------+   +--+------------+   +----------------+
                          | VLAN 10 Sales  |   | VLAN 20 IT    |   | VLAN 30 Servers|
                          | 192.168.10.0/24|   |192.168.20.0/24|   |192.168.30.0/24 |
                          | PC0, PC1 (DHCP)|   |PC2, PC3 (DHCP)|   | Server2 (static)|
                          +----------------+   +---------------+   +----------------+

















                          Protocol / Feature	Purpose in this network
802.1Q Trunking	Carries tagged traffic for VLANs 10, 20, and 30 over the single link between Switch0 (Fa0/1) and Router1 (Gig0/2).
VLANs	Segments Sales, IT, and Servers into separate broadcast/security domains on the same physical switch.
Router-on-a-Stick (sub-interfaces)	Allows Router1 to route between VLANs using one physical interface split into Gig0/2.10, .20, and .30, each acting as that VLAN's default gateway.
DHCP	Automatically assigns IP/subnet/gateway/DNS to hosts in Sales and IT, removing manual configuration.
Static addressing	Used for Server2 and Server1, since servers need predictable, unchanging addresses.
NAT (PAT / overload)	Translates internal private addresses (192.168.x.x) to Router1's public WAN IP so internal hosts can reach the simulated internet (Server1 at 8.8.8.10).
Static default routing	Router1 sends all non-local traffic to Router0 (the ISP) via ip route 0.0.0.0 0.0.0.0 203.0.113.2.
