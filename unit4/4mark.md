# Transport Layer - Complete 4 Marks Study Guide

## 1. Different Layers of Reliable Connection Protocol and Operation

### Layers of TCP (Reliable Connection Protocol):

```
Application Layer
       ↓
┌─────────────────────────────────┐
│    TRANSPORT LAYER (TCP)        │
├─────────────────────────────────┤
│  Connection Management Layer    │
│  • Connection Establishment     │
│  • Connection Termination       │
├─────────────────────────────────┤
│  Flow Control Layer             │
│  • Sliding Window               │
│  • Buffer Management            │
├─────────────────────────────────┤
│  Error Control Layer            │
│  • Sequence Numbers             │
│  • Acknowledgments              │
│  • Retransmission               │
├─────────────────────────────────┤
│  Congestion Control Layer       │
│  • Slow Start                   │
│  • Congestion Avoidance         │
└─────────────────────────────────┘
       ↓
Network Layer (IP)
```

### Operation Flow:
1. **Connection Establishment**: Three-way handshake
2. **Data Transfer**: Reliable, ordered delivery
3. **Flow Control**: Prevents receiver overflow
4. **Error Control**: Detects and corrects errors
5. **Congestion Control**: Manages network load
6. **Connection Termination**: Four-way handshake

---

## 2. UDP Header Format (Unreliable Connectionless Transport Protocol)

### UDP Header Structure:
```
 0      7 8     15 16    23 24    31
┌─────────┬─────────┬─────────┬─────────┐
│  Source │ Destin. │ Message │         │
│  Port   │  Port   │ Length  │Checksum │
│ (16bits)│(16bits) │(16bits) │(16bits) │
└─────────┴─────────┴─────────┴─────────┘
│                                       │
│            DATA AREA                  │
│                                       │
└───────────────────────────────────────┘
```

### Component Comments:

**1. Source Port (16 bits)**:
- Identifies sending process port number
- Range: 0-65535
- Optional field (can be 0 if no response needed)

**2. Destination Port (16 bits)**:
- Identifies receiving process port number
- Required for proper delivery
- Well-known ports: HTTP(80), DNS(53), DHCP(67)

**3. Message Length (16 bits)**:
- Total length of UDP header + data
- Minimum value: 8 bytes (header only)
- Maximum value: 65535 bytes

**4. Checksum (16 bits)**:
- Error detection for header and data
- Optional in IPv4, mandatory in IPv6
- Computed using pseudo-header

---

## 3. UDP Message Queue Techniques

### 1. First-In-First-Out (FIFO) Queue:
```
Incoming Messages: [M3] [M2] [M1] → Queue → [M1] [M2] [M3] → Application
                   Newest          Oldest    Oldest    Newest
```

### 2. Priority-Based Queuing:
```
High Priority:    [Critical Data]     → Process First
Medium Priority:  [Normal Data]      → Process Second  
Low Priority:     [Background Data]  → Process Last
```

### 3. Circular Buffer Queue:
```
Buffer: [M1] [M2] [M3] [  ] [  ] [M6] [M7]
         ↑                           ↑
       Head                        Tail
```

### 4. Socket Buffer Management:
- **Receive Buffer**: Stores incoming UDP messages
- **Send Buffer**: Stores outgoing UDP messages
- **Buffer Size**: Configurable (default: 64KB)

### Implementation Example:
```
UDP Socket → Receive Buffer → Message Queue → Application
          ← Send Buffer    ← Message Queue ← Application
```

---

## 4. Functions of Reliable vs Unreliable Transport Protocols

### TCP (Reliable Connection Protocol) Functions:

**1. Connection Management**:
- Establishes connection before data transfer
- Maintains connection state
- Graceful connection termination

**2. Reliable Data Transfer**:
- Guarantees data delivery
- Maintains data order
- Eliminates duplicates

**3. Flow Control**:
- Prevents sender from overwhelming receiver
- Uses sliding window mechanism
- Dynamic window adjustment

**4. Error Control**:
- Detects corrupted/lost data
- Automatic retransmission
- Sequence number management

**5. Congestion Control**:
- Prevents network overload
- Adaptive sending rate
- Fair bandwidth sharing

### UDP (Unreliable Connectionless Protocol) Functions:

**1. Connectionless Communication**:
- No connection establishment required
- Independent packet delivery
- Stateless operation

**2. Process-to-Process Communication**:
- Port-based addressing
- Multiplexing/demultiplexing
- Socket identification

**3. Basic Error Detection**:
- Checksum for error detection
- No error correction
- Discard corrupted packets

**4. Minimal Overhead**:
- Small header size (8 bytes)
- Fast packet processing
- Low resource consumption

---

## 5. Error Control in TCP with Example

### TCP Error Control Mechanisms:

**1. Sequence Numbers**:
- Each byte has unique sequence number
- Detects missing or out-of-order data

**2. Acknowledgments (ACKs)**:
- Confirms receipt of data
- Cumulative acknowledgment
- Duplicate ACKs signal problems

**3. Checksums**:
- Detects corrupted data
- Covers header and data
- Uses pseudo-header

**4. Timers and Retransmission**:
- Retransmit timeout (RTO)
- Exponential backoff
- Fast retransmit

### Example Scenario:
```
Sender:    Send Seq=100, Data="Hello"
          ↓
Network:   Packet corrupted during transmission
          ↓
Receiver:  Checksum fails, packet discarded
          ↓
Sender:    Timeout occurs, retransmit Seq=100
          ↓
Receiver:  Packet received correctly, send ACK=105
          ↓
Sender:    Receive ACK=105, continue with Seq=105
```

### Detailed Process:
1. **Transmission**: Data sent with sequence number
2. **Error Detection**: Receiver detects corruption
3. **No ACK Sent**: Corrupted packet discarded
4. **Timeout**: Sender timer expires
5. **Retransmission**: Same data sent again
6. **Success**: Clean packet acknowledged

---

## 6. Error Control in UDP with Example

### UDP Error Control:

**Limited Error Control**:
- Only checksum for error detection
- No acknowledgments
- No retransmission
- No sequence numbers

### UDP Error Handling Process:
```
Sender → UDP Packet + Checksum → Network → Receiver
                                    ↓
                               Checksum Check
                                    ↓
                          Pass: Deliver to Application
                          Fail: Discard Packet (Silent Drop)
```

### Example Scenario:
```
DNS Client sends query to DNS Server:

1. Client creates UDP packet:
   Source Port: 5000
   Dest Port: 53
   Data: "www.google.com"
   Checksum: Calculated

2. Network corrupts packet during transmission

3. DNS Server receives packet:
   - Calculates checksum
   - Detects error
   - Silently discards packet

4. Client waits for response:
   - No response received
   - Application timeout occurs
   - Client may retry (application-level)
```

### Key Differences from TCP:
- **No automatic retransmission**
- **No acknowledgments**
- **Application must handle errors**
- **Silent packet dropping**

---

## 7. Three-Way Handshaking for Connection Establishment

### Is Three-Way Handshaking Necessary? **YES**

### Justification:

**1. Synchronization**: Both sides must agree on initial sequence numbers
**2. Full-Duplex Setup**: Both directions must be established
**3. Resource Allocation**: Buffers and connection state setup
**4. Duplicate Prevention**: Prevents old connection requests

### Three-Way Handshake Process:
```
Client                                Server
  │                                     │
  │─────── SYN (Seq=X) ────────────────→│  Step 1: Connection Request
  │                                     │
  │←─── SYN+ACK (Seq=Y, Ack=X+1) ──────│  Step 2: Accept & Request
  │                                     │
  │─────── ACK (Ack=Y+1) ──────────────→│  Step 3: Confirm Connection
  │                                     │
  │         Connection Established       │
```

### Why Not Two-Way?
**Problem**: Server can't distinguish between:
- New connection request
- Delayed duplicate from old connection

### Example Problem with Two-Way:
```
1. Client sends SYN(100) → Lost
2. Client sends SYN(200) → Received, connection established
3. Old SYN(100) arrives later → Would create duplicate connection!
```

### Three-Way Solves This:
- Client must respond with correct ACK
- Old requests won't get proper ACK response
- Prevents duplicate connections

---

## 8. Reliable vs Unreliable Flooding

### Reliable Flooding:

**Characteristics**:
- Acknowledgment required for each packet
- Retransmission on failure
- Maintains delivery guarantees
- Higher overhead

**Process**:
```
Node A → Flood to B,C,D → Each sends ACK → A confirms delivery
       → If no ACK → Retransmit → Continue until ACK received
```

**Example**:
```
Routing Update Flooding:
Router A: Send update to B,C,D
Router B: Receive update → Send ACK to A → Forward to neighbors
Router C: Packet lost → A timeout → Retransmit to C
Router D: Receive update → Send ACK to A → Forward to neighbors
```

### Unreliable Flooding:

**Characteristics**:
- No acknowledgments required
- No retransmission
- Best-effort delivery
- Lower overhead

**Process**:
```
Node A → Flood to B,C,D → No ACK expected → Continue with next packet
       → Some may be lost → No recovery mechanism
```

**Example**:
```
Broadcast Message:
Router A: Send broadcast to all neighbors
Neighbors: Receive and forward (if not already seen)
          → Some packets may be lost
          → No retransmission
          → Faster but less reliable
```

### Key Differences:
| Feature | Reliable Flooding | Unreliable Flooding |
|---------|------------------|-------------------|
| ACK Required | Yes | No |
| Retransmission | Yes | No |
| Delivery Guarantee | High | None |
| Overhead | High | Low |
| Speed | Slower | Faster |
| Use Case | Critical updates | Non-critical broadcasts |

---

## 9. Flow Control: Eight Segments Exchange

### Scenario: Client-Server Communication with Flow Control

**Initial Setup**:
- Client Window Size: 4 segments
- Server Window Size: 4 segments
- Segment Size: 1KB each

### Step-by-Step Exchange:

```
Time | Client Action          | Server Action           | Client Window | Server Window
-----|------------------------|-------------------------|---------------|---------------
T1   | Send Seg 1,2,3,4      | -                       | [1,2,3,4]     | [ ]
T2   | Wait for ACK          | Receive 1,2,3,4         | [1,2,3,4]     | [1,2,3,4]
T3   | -                     | Process 1,2             | [1,2,3,4]     | [3,4, , ]
T4   | -                     | Send ACK 3, Win=2       | [1,2,3,4]     | [3,4, , ]
T5   | Receive ACK 3         | -                       | [3,4,5,6]     | [3,4, , ]
T6   | Send Seg 5,6          | -                       | [3,4,5,6]     | [3,4, , ]
T7   | -                     | Receive 5,6             | [3,4,5,6]     | [3,4,5,6]
T8   | -                     | Process all, ACK 7, Win=4| [3,4,5,6]    | [ , , , ]
```

### Detailed Flow:
1. **T1**: Client sends 4 segments (fills window)
2. **T2**: Server receives all 4 segments
3. **T3**: Server processes 2 segments, has space for 2 more
4. **T4**: Server sends ACK=3 (next expected), Window=2
5. **T5**: Client slides window, can send 2 more segments
6. **T6**: Client sends segments 5,6
7. **T7**: Server receives segments 5,6
8. **T8**: Server processes all data, advertises full window

### Flow Control Benefits:
- Prevents buffer overflow
- Adapts to receiver processing speed
- Maintains reliable delivery

---

## 10. Congestion Control Mechanism in Transport Layer

### TCP Congestion Control Components:

### 1. Slow Start:
```
Initial: cwnd = 1 MSS
For each ACK: cwnd = cwnd + 1 MSS
Result: Exponential growth

RTT 1: cwnd = 1  → Send 1 segment
RTT 2: cwnd = 2  → Send 2 segments  
RTT 3: cwnd = 4  → Send 4 segments
RTT 4: cwnd = 8  → Send 8 segments
```

### 2. Congestion Avoidance:
```
When cwnd >= ssthresh:
For each ACK: cwnd = cwnd + (1/cwnd)
Result: Linear growth (1 MSS per RTT)
```

### 3. Fast Retransmit:
```
Duplicate ACK count = 3 → Retransmit immediately
Don't wait for timeout
```

### 4. Fast Recovery:
```
After fast retransmit:
ssthresh = cwnd / 2
cwnd = ssthresh + 3
Continue congestion avoidance
```

### Congestion Control Algorithm:
```
State Machine:
SLOW START → (cwnd >= ssthresh) → CONGESTION AVOIDANCE
     ↑                                       ↓
     ←─────── (timeout/3 dup ACKs) ──────────┘
```

### Example Scenario:
```
cwnd = 1, ssthresh = 64
RTT 1: Send 1, cwnd = 2
RTT 2: Send 2, cwnd = 4  
RTT 3: Send 4, cwnd = 8
...
RTT 6: cwnd = 64 (reached ssthresh)
RTT 7: cwnd = 65 (linear increase)
RTT 8: cwnd = 66
Packet loss detected → cwnd = 33, ssthresh = 33
```

---

## 11. TCP Congestion Control - AIMD (Additive Increase Multiplicative Decrease)

### AIMD Concept:
**Additive Increase**: Slowly increase sending rate when no congestion
**Multiplicative Decrease**: Rapidly decrease when congestion detected

### Algorithm Details:

**Additive Increase**:
```
Every RTT without loss:
cwnd = cwnd + 1 MSS
```

**Multiplicative Decrease**:
```
When congestion detected:
ssthresh = cwnd / 2
cwnd = ssthresh
```

### AIMD Graph:
```
Congestion Window
        ↑
   16   |     /\
   14   |    /  \
   12   |   /    \
   10   |  /      \
    8   | /        \
    6   |/          \
    4   |            \
    2   |             \
    0   |______________\________→ Time
        Loss          Loss
```

### Example Timeline:
```
Time | Event | cwnd | Action
-----|-------|------|--------
T1   | Start | 1    | Slow start
T2   | ACK   | 2    | Double
T3   | ACK   | 4    | Double  
T4   | ACK   | 8    | Double
T5   | Reach | 8    | Switch to AIMD
T6   | ACK   | 9    | Add 1
T7   | ACK   | 10   | Add 1
T8   | Loss  | 5    | Halve (10/2)
T9   | ACK   | 6    | Add 1
```

### Benefits of AIMD:
- **Fairness**: Multiple connections converge to fair share
- **Efficiency**: Uses available bandwidth
- **Responsiveness**: Quick response to congestion
- **Stability**: Oscillates around optimal point

---

## 12. Functions of Transport Layer

### Primary Functions:

**1. Process-to-Process Communication**:
- Uses port numbers for identification
- Socket addressing (IP + Port)
- Multiplexing multiple applications

**2. Segmentation and Reassembly**:
- Breaks large messages into segments
- Adds sequence numbers
- Reassembles at destination

**3. Connection Management** (TCP only):
- Connection establishment
- Connection maintenance  
- Connection termination

**4. Reliable Data Transfer** (TCP):
- Error detection and correction
- Acknowledgments and retransmission
- Duplicate elimination

**5. Flow Control**:
- Prevents receiver buffer overflow
- Sliding window mechanism
- Rate matching between sender/receiver

**6. Congestion Control**:
- Prevents network overload
- Adapts to network conditions
- Fair bandwidth sharing

**7. Error Control**:
- Checksum calculation
- Error detection
- Recovery mechanisms

### Service Models:
```
TCP Services:
- Reliable, connection-oriented
- Full-duplex communication
- Flow and congestion control

UDP Services:  
- Unreliable, connectionless
- Simple, fast communication
- Minimal overhead
```

---

## 13. QoS (Quality of Service) and Types of QoS Tools

### QoS Definition:
Quality of Service refers to the ability to provide different priority levels and guarantee certain performance characteristics for network traffic.

### QoS Parameters:
- **Bandwidth**: Data rate available
- **Delay**: Time for packet to reach destination
- **Jitter**: Variation in delay
- **Packet Loss**: Percentage of lost packets

### Types of QoS Tools:

### 1. Traffic Shaping Tools:
**Leaky Bucket**:
```
Input (Variable) → [Bucket] → Output (Constant Rate)
                     ↓
                 Overflow
```

**Token Bucket**:
```
Token Generator → [Token Bucket] → Allow Transmission
                      ↓
              Packet Queue → Drop if no tokens
```

### 2. Queuing Algorithms:
**Priority Queuing**:
```
High Priority:    [P1] [P2] → Process First
Medium Priority:  [P3] [P4] → Process Second
Low Priority:     [P5] [P6] → Process Last
```

**Weighted Fair Queuing (WFQ)**:
```
Queue 1 (Weight 3): Gets 3/6 of bandwidth
Queue 2 (Weight 2): Gets 2/6 of bandwidth  
Queue 3 (Weight 1): Gets 1/6 of bandwidth
```

### 3. Congestion Control:
- **Random Early Detection (RED)**
- **Explicit Congestion Notification (ECN)**
- **Traffic policing**

### 4. Resource Reservation:
- **IntServ (Integrated Services)**
- **DiffServ (Differentiated Services)**
- **MPLS (Multi-Protocol Label Switching)**

---

## 14. Port and Segment Definitions

### Port:
**Definition**: A 16-bit number that identifies a specific process or application on a host.

**Characteristics**:
- Range: 0 to 65,535
- Enables process-to-process communication
- Combined with IP address forms socket address

**Port Categories**:
```
Well-Known Ports (0-1023):
- HTTP: 80
- HTTPS: 443  
- FTP: 21
- SSH: 22
- DNS: 53

Registered Ports (1024-49151):
- Application-specific
- Assigned by IANA

Dynamic/Private Ports (49152-65535):
- Temporary assignments
- Client-side connections
```

### Segment:
**Definition**: A unit of data at the Transport Layer, consisting of header plus application data.

**Components**:
```
┌─────────────────────────────┐
│     Transport Header        │  ← Protocol-specific header
├─────────────────────────────┤
│                             │
│     Application Data        │  ← Payload from upper layer
│                             │
└─────────────────────────────┘
```

**Types**:
- **TCP Segment**: TCP header + data
- **UDP Datagram**: UDP header + data

**Size Considerations**:
- **MSS (Maximum Segment Size)**: Largest amount of data in TCP segment
- **MTU (Maximum Transmission Unit)**: Maximum frame size at network layer
- Relationship: MSS = MTU - IP Header - TCP Header

---

## 15. Techniques to Improve QoS

### 1. Integrated Services (IntServ):
**Resource Reservation Protocol (RSVP)**:
```
Application → RSVP Request → Network → Reserve Resources → Guaranteed QoS
```

**Components**:
- **Path Messages**: Sender to receiver
- **Resv Messages**: Receiver to sender  
- **Resource Reservation**: End-to-end path

### 2. Differentiated Services (DiffServ):
**Traffic Classification**:
```
DSCP (Differentiated Services Code Point):
EF (Expedited Forwarding): Voice traffic
AF (Assured Forwarding): Video traffic  
BE (Best Effort): Data traffic
```

### 3. Traffic Engineering:
**MPLS (Multi-Protocol Label Switching)**:
```
Ingress Router → Label Switch Path → Egress Router
              ↓
         Traffic Engineering
```

### 4. Queue Management:
**Active Queue Management**:
- **RED (Random Early Detection)**
- **WRED (Weighted RED)**
- **BLUE Algorithm**

### 5. Bandwidth Management:
**Traffic Shaping**:
```
Rate Limiting:
Input Rate > Allowed Rate → Buffer or Drop
Input Rate ≤ Allowed Rate → Forward
```

**Traffic Policing**:
- Monitor traffic rates
- Enforce traffic contracts
- Drop or mark violating traffic

### 6. Application-Level Techniques:
**Adaptive Applications**:
- **Video**: Adjust bitrate based on network conditions
- **Audio**: Change codec quality
- **Data**: Compress content

**Forward Error Correction (FEC)**:
```
Original Data + Redundant Data → Receiver can correct errors
```

### 7. Network-Level Improvements:
**Load Balancing**:
- Distribute traffic across multiple paths
- Prevent bottlenecks
- Improve overall performance

**Caching and CDN**:
- Store content closer to users
- Reduce network load
- Improve response time

---

## Exam Strategy for 4 Marks Questions:

### Answer Structure (4 marks):
1. **Definition/Introduction** (1 mark)
2. **Main concept/diagram** (2 marks)  
3. **Example/comparison** (1 mark)

### Writing Tips:
- **Start with clear definition**
- **Use diagrams wherever possible**
- **Provide concrete examples**
- **Compare different approaches**
- **Use proper technical terminology**
- **Keep explanations concise but complete**

### Common Mistakes to Avoid:
- Don't just list points without explanation
- Don't skip examples
- Don't forget to draw diagrams for protocol questions
- Don't mix up TCP and UDP characteristics

This guide covers all concepts you need to score full marks. Practice explaining each topic clearly and you'll be well-prepared!