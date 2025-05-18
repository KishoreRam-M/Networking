# 🔐 Networking Basics: TCP/UDP Ports, Sockets, and Firewalls

---

## ❓ 1. What are TCP/UDP Ports?

### ✅ What:

* **Ports** are **logical communication endpoints** that identify specific services or processes running on a device.
* They operate at the **Transport Layer** using:

  * **TCP** (Transmission Control Protocol)
  * **UDP** (User Datagram Protocol)

### ⚙️ How they work:

* Every connection includes:

  * **Source IP + Source Port**
  * **Destination IP + Destination Port**
* This helps the OS route data to the right application.

### 🎯 Example:

| Service     | Protocol | Port |
| ----------- | -------- | ---- |
| Web (HTTP)  | TCP      | 80   |
| Web (HTTPS) | TCP      | 443  |
| DNS         | UDP      | 53   |

### 📌 Key Notes:

* Port numbers range: `0–65535`

  * **0–1023**: Well-known ports (HTTP, HTTPS, SSH, etc.)
  * **1024–49151**: Registered ports
  * **49152–65535**: Dynamic/private ports
* **TCP** is connection-oriented.
* **UDP** is connectionless and faster but less reliable.

---

## 🔐 2. What Port Does HTTPS Use?

* **HTTPS** = Secure version of HTTP.
* Uses **TCP Port 443**
* Provides **encryption** via **SSL/TLS**
* Example: When visiting `https://example.com`, the browser communicates over port **443**.

---

## ⚖️ 3. Difference Between Port and Socket

| Feature    | **Port**                       | **Socket**                              |
| ---------- | ------------------------------ | --------------------------------------- |
| Definition | Logical number for services    | Combination of **IP + Port + Protocol** |
| Example    | `80` for HTTP                  | `192.168.1.10:80 (TCP)`                 |
| Purpose    | Identifies **where** data goes | Defines the **communication endpoint**  |
| Layer      | **Transport Layer**            | Transport + Network Layer info          |

> 🔄 **In short**: A **port** identifies the service, while a **socket** is the full address used in communication.

---

## 🛡️ 4. How to Secure Open Ports?

### ✅ Best Practices:

1. **Close Unused Ports**

   * Use `firewall` or `netstat` to identify and block unused ports.
2. **Use Secure Protocols**

   * Replace **Telnet** with **SSH**
   * Replace **HTTP** with **HTTPS**
3. **Port Knocking**

   * Ports stay closed unless a secret knock pattern is received.
4. **Change Default Ports**

   * Avoid predictable defaults (e.g., use SSH on `2222` instead of `22`)
5. **Firewalls & IDS**

   * Use tools like **UFW**, **iptables**, or enterprise firewalls.
6. **Run Port Scans**

   * Tools: `nmap`, `netstat`, `Wireshark`
7. **Enable Logging & Alerts**

   * Detect suspicious access in real-time.

---

## 🔥 5. How Does a Firewall Work with Ports?

### ✅ What is a Firewall?

* A **firewall** is a security system that **monitors and filters** traffic based on rules.

### ⚙️ How It Works:

* Controls traffic using:

  * **Port numbers**
  * **IP addresses**
  * **Protocols**

### 🎯 Example Rules:

```txt
ALLOW TCP PORT 80      # HTTP
ALLOW TCP PORT 22      # SSH
DENY  TCP PORT 23      # Telnet (unsecured)
```

### 🧱 Types of Firewalls:

| Type              | Description                                     |
| ----------------- | ----------------------------------------------- |
| **Host-based**    | Software on a computer (e.g., Windows Firewall) |
| **Network-based** | Hardware firewall on routers/gateways           |

---

## 📘 Summary Cheat Sheet

| Term         | Description                                   |
| ------------ | --------------------------------------------- |
| **Port**     | Identifies a service (e.g., port 80 for HTTP) |
| **Socket**   | IP + Port + Protocol (full address)           |
| **Firewall** | Security system for traffic filtering         |
| **TCP/UDP**  | Transport protocols (reliable vs fast)        |
