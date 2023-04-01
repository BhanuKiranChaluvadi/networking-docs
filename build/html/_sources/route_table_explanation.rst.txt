Route Table Explanation
=======================

The route table shows the routing configuration for a Linux system with
multiple network interfaces. Each line represents a routing rule, describing
how to reach a specific network destination.

Route Table Entries
-------------------
.. code-block:: bash

  $ ip route show
  default via 10.53.252.1 dev eno1 proto dhcp metric 100
  10.53.252.0/22 dev eno1 proto kernel scope link src 10.53.253.141 metric 100
  169.254.0.0/16 dev eno1 scope link metric 1000
  172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
  172.20.0.0/16 dev br-f51682b1a7dc proto kernel scope link src 172.20.0.1

1. Default Gateway
^^^^^^^^^^^^^^^^^^^

::
  default via 10.53.252.1 dev eno1 proto dhcp metric 100

- **default**: The default route.
- **via 10.53.252.1**: The IP address of the gateway.
- **dev eno1**: The network interface to use (eno1).
- **proto dhcp**: The route was learned through DHCP.
- **metric 100**: Priority assigned to this route. Lower values have
  higher priority.

**Example**: If the system needs to reach an IP address like 8.8.8.8
(not explicitly defined in the routing table), it will use the default
route and forward the packet to 10.53.252.1 via the eno1 interface.

2. Local Network Route
^^^^^^^^^^^^^^^^^^^^^^

::
  10.53.252.0/22 dev eno1 proto kernel scope link src 10.53.253.141 metric 100

- **10.53.252.0/22**: Destination network address and subnet mask
  (255.255.252.0).
- **dev eno1**: The network interface to use (eno1).
- **proto kernel**: The route was added by the kernel.
- **scope link**: The destination is directly reachable.
- **src 10.53.253.141**: Source IP address used when sending packets.
- **metric 100**: Priority assigned to this route.

**Example**: If the system needs to reach an IP address like 10.53.252.5,
it will use this route and send the packet directly via the eno1
interface with a source IP of 10.53.253.141.

3. Link-Local Network Route
^^^^^^^^^^^^^^^^^^^^^^^^^^^

::
  169.254.0.0/16 dev eno1 scope link metric 1000

- **169.254.0.0/16**: Destination network address and subnet mask
  (255.255.0.0) for link-local addresses (RFC 3927).
- **dev eno1**: The network interface to use (eno1).
- **scope link**: The destination is directly reachable.
- **metric 1000**: Priority assigned to this route.

**Example**: If the system needs to reach an IP address like 169.254.1.2,
it will use this route and send the packet directly via the eno1 interface.

4. Docker Network Route
^^^^^^^^^^^^^^^^^^^^^^^

::
  172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown

- **172.17.0.0/16**: Destination network address and subnet mask (255.255.0.0).
- **dev docker0**: The network interface to use (docker0).
- **proto kernel**: The route was added by the kernel.
- **scope link**: The destination is directly reachable.
- **src 172.17.0.1**: Source IP address used when sending packets.
- **linkdown**: The network interface (docker0) is currently down or
  not operational.

**Example**: If the system needs to reach an IP address like 172.17.0.2
and the docker0 interface is up, it will use this route and send the packet
directly via the docker0 interface with a source IP of 172.17.0.1.
However, since the interface is currently down

5. Bridge Network Route
^^^^^^^^^^^^^^^^^^^^^^^

::
  172.20.0.0/16 dev br-f51682b1a7dc proto kernel scope link src 172.20.0.1

- **172.20.0.0/16**: Destination network address and subnet mask (255.255.0.0).
- **dev br-f51682b1a7dc**: The network interface to use (br-f51682b1a7dc).
- **proto kernel**: The route was added by the kernel.
- **scope link**: The destination is directly reachable.
- **src 172.20.0.1**: Source IP address used when sending packets.

**Example**: If the system needs to reach an IP address like 172.20.0.2,
it will use this route and send the packet directly via the br-f51682b1a7dc
interface with a source IP of 172.20.0.1.

Summary
^^^^^^^

This route table defines the routing behavior for a system with multiple
network interfaces, including a physical interface (eno1), a Docker interface
(docker0), and a bridge interface (br-f51682b1a7dc). The table specifies how
to reach different network destinations, whether they are local networks,
link-local networks, or external networks.
