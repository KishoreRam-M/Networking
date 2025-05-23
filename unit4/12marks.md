# Complete Transport Layer Protocol Study Guide

## 1. General Features of Reliable Message-Oriented Transport Layer Protocol

### Key Features:

**1. Connection Management**
- Establishes, maintains, and terminates connections
- Three-way handshake for connection establishment
- Proper connection teardown procedures

**2. Error Detection and Correction**
- Checksums to detect corrupted data
- Sequence numbers to detect lost/duplicate packets
- Acknowledgments (ACKs) to confirm receipt

**3. Flow Control**
- Prevents sender from overwhelming receiver
- Uses sliding window mechanism
- Receiver advertises available buffer space

**4. Congestion Control**
- Prevents network overload
- Adapts sending rate based on network conditions
- Uses algorithms like slow start, congestion avoidance

**5. Ordered Delivery**
- Maintains packet sequence using sequence numbers
- Reassembles data in correct order at receiver
- Handles out-of-order packet arrival

**6. Duplicate Detection**
- Uses sequence numbers to identify duplicates
- Discards duplicate packets
- Prevents data corruption from duplicates

---

## 2. GO-BACK-N Protocol

### Concept:
Go-Back-N is an ARQ (Automatic Repeat reQuest) protocol where sender can transmit multiple frames before receiving acknowledgments, but must retransmit all frames from the first unacknowledged frame when an error occurs.

### Key Features:
- **Window Size (N)**: Maximum number of unacknowledged frames
- **Sequence Numbers**: 0 to 2^m - 1 (where m is number of bits)
- **Cumulative ACK**: ACK for frame n acknowledges all frames up to n

### Example:
```
Sender Window Size = 4
Sequence Numbers: 0, 1, 2, 3, 4, 5, 6, 7, 0, 1...

Time 1: Send frames 0, 1, 2, 3
Time 2: Receive ACK 1 (frames 0,1 acknowledged)
        Send frames 4, 5
Time 3: Frame 2 lost/corrupted
Time 4: Receive ACK 1 again (duplicate, frame 2 not received)
Time 5: Timeout occurs
        Go back and retransmit frames 2, 3, 4, 5
```

### Advantages:
- Simple receiver implementation
- Efficient for low error rates

### Disadvantages:
- Wasteful bandwidth when errors occur
- Retransmits correctly received frames

---

## 3. SCTP (Stream Control Transmission Protocol) Services

### Services Offered to Application Layer:

**1. Multi-streaming**
- Multiple independent streams within single association
- Eliminates head-of-line blocking
- Each stream has independent sequence numbers

**2. Multi-homing**
- Single association can span multiple IP addresses
- Automatic failover between addresses
- Improved fault tolerance

**3. Message-Oriented**
- Preserves message boundaries (unlike TCP's byte stream)
- Applications send discrete messages
- No need for application-level framing

**4. Congestion Control**
- Similar to TCP congestion control
- Per-destination congestion control
- Slow start and congestion avoidance

**5. Flow Control**
- Receiver flow control using advertised window
- Prevents buffer overflow at receiver

**6. Reliable Delivery**
- Sequence numbers and acknowledgments
- Selective acknowledgments (SACK)
- Retransmission of lost data

**7. Partial Reliability (Extension)**
- Some data can be sent with relaxed reliability
- Time-based or retransmission-based limits

---

## 4. Flow Control Algorithms

### 1. Stop-and-Wait
**Concept**: Send one frame, wait for ACK before sending next.

**Example**:
```
Sender: Send Frame 0 → Wait for ACK 0 → Send Frame 1 → Wait for ACK 1
Receiver: Receive Frame 0 → Send ACK 0 → Receive Frame 1 → Send ACK 1
```

**Advantages**: Simple, no buffer issues
**Disadvantages**: Very inefficient, low utilization

### 2. Sliding Window
**Concept**: Maintain window of frames that can be sent without ACK.

**Example** (Window size = 3):
```
Send Window: [0,1,2] | 3,4,5,6...
Send frames 0,1,2
Receive ACK 0: Window slides → [1,2,3] | 4,5,6...
Send frame 3
Receive ACK 1: Window slides → [2,3,4] | 5,6,7...
```

### 3. Credit-Based Flow Control
**Concept**: Receiver grants credits (permission) to sender for number of frames.

**Example**:
```
Initial: Receiver grants 5 credits
Sender: Sends 3 frames (2 credits remaining)
Receiver: Processes frames, grants 3 more credits (5 total)
Sender: Can now send 5 more frames
```

---

## 5. UDP Process-to-Process Communication using Socket Address

### Proof of UDP's Process-to-Process Communication:

**1. Socket Address Structure**:
```
Socket Address = (IP Address, Port Number)
Example: (192.168.1.100, 53) for DNS server
```

**2. Port Number Space**:
- 16-bit port numbers (0-65535)
- Each process binds to unique port on host
- Well-known ports: 0-1023 (HTTP:80, DNS:53)
- Dynamic ports: 49152-65535

**3. UDP Header Format**:
```
|Source Port (16 bits)|Destination Port (16 bits)|
|Length (16 bits)     |Checksum (16 bits)        |
|Data...                                         |
```

**4. Communication Process**:
```
Process A (192.168.1.10:5000) → Process B (192.168.1.20:8080)
UDP adds source and destination socket addresses
Network layer uses IP addresses for routing
Process B receives data using destination port 8080
```

**5. Multiplexing/Demultiplexing**:
- **Multiplexing**: Multiple processes send data through single UDP
- **Demultiplexing**: UDP delivers data to correct process using port number

---

## 6. Selective Repeat Protocol

### Concept:
Selective Repeat allows receiver to acknowledge individual frames. Only lost or corrupted frames are retransmitted, not entire window.

### Key Features:
- Individual frame acknowledgment
- Receiver has buffer for out-of-order frames
- Sender maintains timer for each frame
- Window size ≤ 2^(m-1) where m is sequence number bits

### Example:
```
Window Size = 4, Sequence Numbers: 0-7

Time 1: Send frames 0,1,2,3
Time 2: Frame 1 lost, frames 0,2,3 received
Time 3: Receiver sends ACK 0, NAK 1, ACK 2, ACK 3
        Receiver buffers frames 2,3 (out of order)
Time 4: Sender retransmits only frame 1
Time 5: Frame 1 received, all frames 0,1,2,3 delivered to application
        Send frames 4,5,6,7
```

### Buffer Management:
```
Receiver Buffer:
[0] ✓ [1] ✗ [2] ✓ [3] ✓ [4] Empty [5] Empty

After frame 1 retransmission:
[0] Delivered [1] ✓ [2] ✓ [3] ✓ → All delivered in order
```

### Advantages:
- Efficient bandwidth utilization
- Only retransmits necessary frames

### Disadvantages:
- Complex receiver implementation
- Requires buffering at receiver

---

## 7. TCP Packet Header Format

### Complete TCP Header Structure (20 bytes minimum):

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Field Descriptions:

**1. Source Port (16 bits)**:
- Port number of sending process
- Range: 0-65535
- Example: HTTP client might use port 45678

**2. Destination Port (16 bits)**:
- Port number of receiving process
- Example: HTTP server uses port 80

**3. Sequence Number (32 bits)**:
- Position of first data byte in this segment
- Used for ordering and duplicate detection
- Initial value chosen randomly during connection setup

**4. Acknowledgment Number (32 bits)**:
- Next sequence number expected by receiver
- Valid only when ACK flag is set
- Cumulative acknowledgment

**5. Data Offset (4 bits)**:
- TCP header length in 32-bit words
- Minimum value: 5 (20 bytes)
- Maximum value: 15 (60 bytes with options)

**6. Reserved (6 bits)**:
- Must be zero
- Reserved for future use

**7. Control Flags (6 bits)**:
- **URG**: Urgent pointer field significant
- **ACK**: Acknowledgment field significant
- **PSH**: Push function (deliver immediately)
- **RST**: Reset connection
- **SYN**: Synchronize sequence numbers
- **FIN**: No more data from sender

**8. Window (16 bits)**:
- Flow control - available buffer space
- Maximum: 65,535 bytes
- Can be scaled using window scaling option

**9. Checksum (16 bits)**:
- Error detection for header and data
- Computed over pseudo-header, TCP header, and data

**10. Urgent Pointer (16 bits)**:
- Points to urgent data
- Valid only when URG flag is set

**11. Options (0-40 bytes)**:
- Maximum segment size, window scaling, timestamps, etc.
- Padded to 32-bit boundary

---

## 8. TCP Congestion Control Mechanisms

### 1. Slow Start

**Concept**: Start with small congestion window, double it every RTT until threshold reached.

**Algorithm**:
```
Initial: cwnd = 1 MSS, ssthresh = 64KB
For each ACK received: cwnd = cwnd + 1 MSS
Continue until cwnd >= ssthresh
```

**Example**:
```
RTT 1: cwnd = 1, send 1 segment
RTT 2: cwnd = 2, send 2 segments  
RTT 3: cwnd = 4, send 4 segments
RTT 4: cwnd = 8, send 8 segments
...until cwnd reaches ssthresh
```

### 2. Congestion Avoidance

**Concept**: After slow start, increase cwnd linearly to probe for available bandwidth.

**Algorithm**:
```
When cwnd >= ssthresh:
For each ACK: cwnd = cwnd + (1 MSS / cwnd)
Result: cwnd increases by 1 MSS per RTT
```

**Example**:
```
cwnd = 8 MSS (reached ssthresh)
Receive 8 ACKs: cwnd = 8 + (8 × 1/8) = 9 MSS
Next RTT: cwnd = 9 + (9 × 1/9) = 10 MSS
```

### 3. Fast Retransmit and Fast Recovery

**Fast Retransmit**:
- Retransmit lost segment after 3 duplicate ACKs
- Don't wait for timeout

**Fast Recovery**:
- After fast retransmit, set ssthresh = cwnd/2
- Set cwnd = ssthresh + 3 MSS
- Continue congestion avoidance

**Example**:
```
Normal transmission: cwnd = 16 MSS
Loss detected (3 duplicate ACKs)
Fast Retransmit: Retransmit lost segment
Fast Recovery: 
  ssthresh = 16/2 = 8 MSS
  cwnd = 8 + 3 = 11 MSS
Continue with congestion avoidance
```

---

## 9. Leaky Bucket and Token Bucket Algorithms

### Leaky Bucket Algorithm

**Concept**: Data enters bucket at variable rate, leaves at constant rate. Bucket overflows if input rate > output rate.

**Characteristics**:
- Constant output rate
- Variable input rate
- Smooths bursty traffic

**Example**:
```
Bucket capacity: 10 packets
Output rate: 2 packets/second
Input: 8 packets at t=0, 3 packets at t=1

t=0: Input 8, Output 2, Bucket: 6 packets
t=1: Input 3, Output 2, Bucket: 7 packets  
t=2: Input 0, Output 2, Bucket: 5 packets
t=3: Input 0, Output 2, Bucket: 3 packets
```

**Implementation**:
```
Algorithm:
1. Check if packet fits in bucket
2. If yes, add to bucket
3. If no, drop packet
4. Output packets at constant rate
```

### Token Bucket Algorithm

**Concept**: Tokens generated at constant rate. Each packet needs token to be transmitted. Allows controlled bursts.

**Characteristics**:
- Tokens generated at constant rate
- Bucket stores tokens (up to capacity)
- Packets consume tokens for transmission

**Example**:
```
Token generation: 2 tokens/second
Bucket capacity: 10 tokens
Packet arrival: 5 packets at t=0, 2 packets at t=2

t=0: 10 tokens available, 5 packets arrive
     Consume 5 tokens, transmit 5 packets
     Remaining: 5 tokens
t=1: Generate 2 tokens, total = 7 tokens
t=2: 2 packets arrive, consume 2 tokens
     Transmit 2 packets, remaining: 5 tokens
```

**Implementation**:
```
Algorithm:
1. Generate tokens at constant rate
2. Store tokens in bucket (up to capacity)
3. For each packet:
   - If tokens available, consume token and transmit
   - If no tokens, drop or queue packet
```

### Comparison:
| Feature | Leaky Bucket | Token Bucket |
|---------|--------------|--------------|
| Output Rate | Constant | Variable (bursty) |
| Burst Handling | Smooths bursts | Allows controlled bursts |
| Implementation | Simpler | More complex |
| Traffic Shaping | Better | Good |

---

## 10. QoS Challenges in TCP vs UDP

### TCP QoS Challenges:

**1. Connection-Oriented Nature**:
- Three-way handshake adds latency
- Connection state maintenance overhead
- Graceful connection termination required

**2. Congestion Control Interference**:
- Automatic rate reduction during congestion
- May conflict with application QoS requirements
- Difficult to guarantee bandwidth

**Example**: Video streaming application needs 2 Mbps, but TCP reduces rate to 500 Kbps during congestion.

**3. Head-of-Line Blocking**:
- Lost packets block subsequent data
- Affects real-time applications
- Retransmissions add variable delay

**4. Byte Stream Service**:
- No message boundaries
- Application must handle framing
- Complicates QoS measurement

**5. Reliability vs. Timeliness Trade-off**:
- Retransmissions increase delay
- May deliver data too late for real-time use
- Cannot disable reliability selectively

### UDP QoS Challenges:

**1. No Built-in Flow Control**:
- Can overwhelm receiver
- Application must implement rate control
- Network congestion not automatically handled

**Example**: UDP video streaming at 10 Mbps on 1 Mbps link causes packet loss.

**2. No Congestion Control**:
- Doesn't respond to network congestion
- Can worsen network conditions
- Requires application-level congestion control

**3. Unreliable Delivery**:
- Packet loss affects QoS
- No automatic retransmission
- Application must handle lost data

**4. Out-of-Order Delivery**:
- Packets may arrive out of sequence
- Affects media playback quality
- Application must reorder if needed

**5. No Connection State**:
- Difficult to maintain per-flow QoS
- No session-based QoS management
- Each packet treated independently

### Key Differences Summary:

**TCP QoS Issues**:
- Automatic mechanisms interfere with QoS
- Reliable but potentially delayed delivery
- Complex state management
- Better for non-real-time applications

**UDP QoS Issues**:
- Lack of automatic mechanisms
- Fast but unreliable delivery  
- Simple but requires application handling
- Better for real-time applications

**QoS Solutions**:
- **For TCP**: Traffic shaping, priority queuing, bandwidth reservation
- **For UDP**: Application-level flow control, FEC (Forward Error Correction), adaptive bitrate
- **Both**: DiffServ, IntServ, MPLS for network-level QoS

---

## Exam Tips for Full Marks:

1. **Always provide examples** - Most concepts become clearer with concrete examples
2. **Draw diagrams** - Especially for protocols like Go-Back-N and Selective Repeat
3. **Explain the 'why'** - Don't just describe what happens, explain why it's designed that way
4. **Compare and contrast** - Show understanding by highlighting differences between approaches
5. **Use proper terminology** - Use technical terms correctly (MSS, RTT, cwnd, etc.)
6. **Show calculations** - For window sizes, timeouts, throughput calculations
7. **Mention advantages and disadvantages** - Shows deeper understanding
8. **Structure your answers** - Use headings and bullet points for clarity