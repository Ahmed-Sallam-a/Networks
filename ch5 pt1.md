### Detailed Summary of Chapter 5: The Network Layer

This chapter delves into the **Network Layer**, which is responsible for **packet forwarding** and **routing** in computer networks. The chapter covers various design issues, services provided to the transport layer, and the implementation of both **connectionless** and **connection-oriented** services. It also compares **virtual-circuit** and **datagram networks**, discusses routing algorithms, and explores congestion control mechanisms.

---

### **1. Network Layer Design Issues**

#### **Store-and-Forward Packet Switching**
- **Definition**: In store-and-forward packet switching, packets are stored briefly in routers before being forwarded to the next hop. This allows for error checking and routing decisions.
- **Environment**: The network layer operates in an environment where packets are transmitted from source to destination via routers, which are connected by LANs and WANs.

#### **Services Provided to the Transport Layer**
- **Independent of Router Technology**: The network layer provides services that are independent of the underlying router technology.
- **Shielding Transport Layer**: The transport layer is shielded from the number, type, and topology of routers.
- **Uniform Numbering Plan**: Network addresses are uniformly numbered, even across LANs and WANs.

---

### **2. Implementation of Connectionless Service**

#### **Datagram Networks**
- **Definition**: In a **connectionless service**, packets (called **datagrams**) are injected into the network individually and routed independently. No advance setup is needed.
- **Routing**: Each packet is routed based on the router's routing table, which can change dynamically based on network conditions (e.g., traffic jams).
- **Example**: In Figure 5.2, packets 1, 2, and 3 follow one route, while packet 4 takes a different route due to updated routing tables.

#### **Routing Algorithm**
- **Definition**: The algorithm that manages routing tables and makes routing decisions is called the **routing algorithm**. It ensures packets are forwarded efficiently based on current network conditions.

---

### **3. Implementation of Connection-Oriented Service**

#### **Virtual-Circuit Networks**
- **Definition**: In a **connection-oriented service**, a path (called a **virtual circuit** or **VC**) is established from the source to the destination before any data packets are sent.
- **Setup and Release**: The connection is set up before data transfer and released afterward. Each packet carries an identifier (VC number) indicating which virtual circuit it belongs to.
- **Conflict Resolution**: Routers may need to replace connection identifiers to avoid conflicts, a process known as **label switching**. **MPLS (MultiProtocol Label Switching)** is an example of a connection-oriented network service.

#### **Example**: In Figure 5.3, multiple hosts (H1 and H3) establish connections to H2, and routers assign different connection identifiers to avoid conflicts.

---

### **4. Comparison of Virtual-Circuit and Datagram Networks**

| **Issue**               | **Datagram Network**                                                                 | **Virtual-Circuit Network**                                                                 |
|--------------------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Circuit Setup**        | Not needed                                                                          | Required                                                                                   |
| **Addressing**           | Each packet contains full source and destination addresses                          | Each packet contains a short VC number                                                     |
| **State Information**    | Routers do not hold state information about connections                             | Each VC requires router table space per connection                                          |
| **Routing**              | Each packet is routed independently                                                 | Route chosen when VC is set up; all packets follow it                                       |
| **Effect of Router Failures** | None, except for packets lost during the crash                                   | All VCs passing through the failed router are terminated                                    |
| **Quality of Service**   | Difficult                                                                           | Easy if resources are allocated in advance                                                 |
| **Congestion Control**   | Difficult                                                                           | Easy if resources are allocated in advance                                                 |

#### **Trade-offs**:
- **Setup Time vs. Address Parsing Time**: Virtual circuits require setup time but simplify packet forwarding. Datagram networks avoid setup but require more complex address parsing.
- **Table Space**: Datagram networks need entries for every possible destination, while virtual-circuit networks need entries only for active VCs.
- **Overhead**: Datagram networks include full destination addresses in each packet, which can be inefficient for short packets.
- **Quality of Service**: Virtual circuits can guarantee QoS and avoid congestion by reserving resources in advance.

---

### **5. Routing Algorithms**

#### **Forwarding vs. Routing**
- **Forwarding**: The process of handling each packet as it arrives, looking up the outgoing line in the routing tables.
- **Routing**: The process of filling in and updating the routing tables. The **routing algorithm** determines how routes are computed.

#### **Desirable Properties of Routing Algorithms**:
- **Correctness**: The algorithm must correctly route packets.
- **Simplicity**: The algorithm should be simple to implement.
- **Robustness**: The algorithm should handle network changes and failures.
- **Stability**: The algorithm should converge to stable routes.
- **Fairness**: The algorithm should treat all users fairly.
- **Efficiency**: The algorithm should minimize resource usage.

#### **Types of Routing Algorithms**:
- **Non-adaptive (Static)**: Routes are computed in advance and do not change.
- **Adaptive (Dynamic)**: Routes change dynamically based on network conditions.

---

### **6. Specific Routing Algorithms**

#### **Shortest Path Algorithm (Dijkstra's Algorithm)**
- **Definition**: Dijkstra's algorithm finds the shortest paths between a source and all destinations in the network.
- **Steps**:
  1. Initialize all labels as tentative.
  2. Mark the source node as permanent.
  3. Update the labels of neighboring nodes.
  4. Repeat until all nodes are permanent.
- **Example**: In Figure 5.7, the algorithm computes the shortest path from node A to D.

#### **Flooding**
- **Definition**: Flooding is a simple routing technique where every incoming packet is sent out on every outgoing line except the one it arrived on.
- **Use Cases**: Flooding is used for broadcasting and is robust in the face of network failures.
- **Hop Counter**: To prevent infinite loops, a hop counter is used to limit the number of hops a packet can take.

#### **Distance Vector Routing**
- **Definition**: Each router maintains a table (vector) of the best-known distance to each destination and the link to use.
- **Updates**: Routers exchange information with neighbors to update their tables.
- **Count-to-Infinity Problem**: Bad news (e.g., a link failure) spreads slowly, causing routing loops. This is mitigated by setting infinity to the longest path plus 1.

#### **Link State Routing**
- **Steps**:
  1. Discover neighbors and learn network addresses.
  2. Set distance/cost metric to each neighbor.
  3. Construct a link state packet (LSP) containing all learned information.
  4. Flood the LSP to all routers.
  5. Compute the shortest path to every other router using Dijkstra's algorithm.
- **Example**: In Figure 5.12, routers build LSPs and flood them to all other routers.

---

### **7. Hierarchical Routing**

- **Definition**: As networks grow, routing tables become large. **Hierarchical routing** divides routers into regions, reducing the size of routing tables.
- **Trade-off**: Hierarchical routing reduces table size but may increase path length.

#### **Example**: In a network with 720 routers, hierarchical routing reduces the number of entries from 720 to 53 (for 24 regions) or 25 (for a three-level hierarchy).

---

### **8. Broadcast and Multicast Routing**

#### **Broadcast Routing**
- **Definition**: Sending a packet to all nodes in the network.
- **Reverse Path Forwarding**: A router forwards a broadcast packet only if it arrives on the preferred path to the source.

#### **Multicast Routing**
- **Definition**: Sending a packet to a group of nodes.
- **Pruning**: The broadcast spanning tree is pruned to remove links that do not lead to group members.

---

### **9. Routing for Mobile Hosts**

- **Home Agent**: A mobile host informs a **home agent** of its current location. The home agent forwards packets to the mobile host.
- **Example**: In Figure 5.19, a mobile host registers its care-of address with the home agent, which tunnels packets to the mobile host.

---

### **10. Congestion Control Algorithms**

#### **Approaches to Congestion Control**
- **Traffic-Aware Routing**: Shift traffic away from hotspots.
- **Admission Control**: Do not set up new virtual circuits if the network is congested.
- **Traffic Throttling**: Reduce traffic when congestion is detected.
- **Load Shedding**: Discard packets when congestion is severe.

#### **Traffic Throttling Mechanisms**
- **Choke Packets**: Routers send choke packets to sources to reduce traffic.
- **Explicit Congestion Notification (ECN)**: Routers mark packets to indicate congestion, and the destination echoes the marks back to the sender.
- **Hop-by-Hop Backpressure**: Congestion signals are sent hop-by-hop to quickly reduce traffic.

#### **Load Shedding**
- **Definition**: When congestion is severe, routers discard packets. The choice of which packets to drop depends on the application (e.g., old packets for file transfer, new packets for real-time media).
- **Random Early Detection (RED)**: Routers maintain a running average of queue lengths and drop packets randomly when the average exceeds a threshold.

---

### **Key Formulas and Definitions**

1. **Dijkstra's Algorithm**:
   - **Formula**: \( d_{new} = a \cdot d_{old} + (1 - a) \cdot s_{new} \)
   - **Explanation**: This formula updates the estimated queueing delay \( d \) using an exponentially weighted moving average (EWMA), where \( a \) determines how fast the router forgets recent history.

2. **Explicit Congestion Notification (ECN)**:
   - **Definition**: Two bits in the IP packet header are used to indicate whether the packet has experienced congestion. The destination echoes the congestion signal back to the sender.

3. **Random Early Detection (RED)**:
   - **Definition**: Routers maintain a running average of queue lengths and drop packets randomly when the average exceeds a threshold.

---

### **Conclusion**

This chapter provides a comprehensive overview of the network layer, covering both theoretical and practical aspects of routing and congestion control. It explains the differences between connectionless and connection-oriented services, discusses various routing algorithms, and explores mechanisms for managing network congestion. The chapter also introduces hierarchical routing and specialized routing techniques for broadcast, multicast, and mobile networks.
