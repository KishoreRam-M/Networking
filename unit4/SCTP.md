# ✅ What is SCTP?

**SCTP (Stream Control Transmission Protocol)** is a **transport layer** network protocol, similar to TCP and UDP.

| Protocol | Features                                                                                                                                     |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **TCP**  | Reliable, connection-based, sends data as a **byte stream**                                                                                  |
| **UDP**  | Fast, message-based, **not reliable**                                                                                                        |
| **SCTP** | Combines the best of both:<br>✔️ Reliable like TCP<br>✔️ Sends **messages** like UDP<br>✔️ Supports **multiple streams** and **multihoming** |

---

# 📜 History

* Developed by **IETF (Internet Engineering Task Force)**, specifically by **TSVWG**.
* Created to carry **telephone signals** over the internet, replacing traditional SS7 systems used in telecom networks.

---

# 🚦 How SCTP Works

* SCTP connects **two endpoints** to send data using **multiple streams**.
* Each stream can carry a different type of data.
* If one stream fails, **others continue without interruption**.
* It is **message-oriented** (unlike TCP’s byte stream).

---

# 🌐 Multihoming in SCTP

* **Multihoming** = One device uses **multiple IP addresses**.
* If one connection fails, SCTP switches to another **without dropping the session**.
* Useful for **high-reliability** systems like telecom networks.

---

# 📦 SCTP Packet Structure

**1. Header (12 bytes):**

* **Source Port**
* **Destination Port**
* **Verification Tag** (identifies the connection)
* **Checksum** (detects errors)

**2. Payload:**

* Contains one or more **chunks** of data (can vary in size and type)

---

# 🔐 Security in SCTP

* SCTP resists:

  * **DoS (Denial of Service)** attacks
  * **Masquerade** attempts (fake users)
  * **Service monopolization**
* Works with existing internet security tools.

---

# ⚙️ Special SCTP Services

Used mainly in **telecom** networks:

* **ASAP** – Aggregate Server Access Protocol
* **BICC** – Bearer-independent Call Control
* **DDP** – Direct Data Placement (Segment & Stream control)
* **Diameter over SCTP**

---

# 🧠 Central Point Architecture in SCTP

* **Before:** All SCTP sessions hashed to the same processor (SPU) – caused overload.
* **After:** **Tag-based hashing** distributes sessions across **multiple SPUs** – improves performance.

---

# 🌟 Advantages of SCTP

✅ **Full-duplex**: Send & receive simultaneously
✅ **Multiple streams**: Failures in one stream don’t affect others
✅ **Message-based**: Complete message transfer
✅ **Flow & congestion control**: Prevents network overload
✅ **Fault tolerance**: Uses **multiple IPs**, avoids total failure
✅ **Secure & Reliable**

---

# 🚫 Limitations of SCTP

* Max **8 IP addresses** per endpoint
* Only supports **Static NAT**
* Limited to **protocol numbers 0–63**
* Sessions **timeout after 30 minutes**
* **Immediate impact** on traffic if configuration is changed

---

# 📱 Applications of SCTP

* **Telephone over Internet**
* **3G/4G/5G mobile networks**
* **SS7 signaling systems**
* **Roaming & RAN (Radio Access Network) security**

---

# 📝 Conclusion

SCTP is a **next-gen transport protocol** that blends the best of TCP and UDP.

✅ Reliable
✅ Message-oriented
✅ Secure
✅ Multi-stream & Multi-IP support

It is perfect for **modern telecom systems** and **real-time communication** needs.
