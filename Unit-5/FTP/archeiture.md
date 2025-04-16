## 🍔 FTP Architecture (Like Ordering Food Online)

FTP = **File Transfer Protocol**, used to move files between your computer and a server.

It uses **two separate connections**:
- One for **talking** (commands)
- One for **sending/receiving files**

---

### 📞 1️⃣ **Control Connection = Telephone Call**

- Imagine you're calling a restaurant to place an order.
- You say things like:
  - “Hello” → `USER`
  - “My password is…” → `PASS`
  - “Can I see the menu?” → `LIST`
  - “I want Pizza” → `RETR`

This is what the **Control Connection** does:
- Used for giving **commands**.
- It stays open during the entire FTP session.
- No actual food (data) is delivered on this line — just **instructions**.

---

### 🚚 2️⃣ **Data Connection = Delivery Van**

- Once you order, a delivery van brings your food.
- When the food is delivered, the van leaves.

This is the **Data Connection**:
- It **opens temporarily** for each file or folder list.
- Used only for **transferring actual files/data**.
- It **closes right after** the data is transferred.

---

### 🧠 Summary Table:

| 🔄 Connection Type   | 📌 Purpose                          | ⏱️ Duration             |
|----------------------|--------------------------------------|-------------------------|
| Control Connection   | Send commands & receive responses    | Stays open (whole time) |
| Data Connection      | Transfer files or folder listings    | Opens/closes each time  |

---
