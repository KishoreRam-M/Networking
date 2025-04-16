---

### 📨 **SMTP Email Sending Process (Step-by-Step)**

---

### **Step 1: Connection Establishment**
- The **SMTP client** (e.g., your mail app) connects to the **SMTP server** on **port 25** (or 587 for secure mail).
- The server responds with a message like:
  ```
  220 mail.example.com Service ready
  ```

---

### **Step 2: Say Hello**
- The client identifies itself using the **HELO** or **EHLO** command:
  ```
  HELO client.example.com
  ```
- Server replies:
  ```
  250 Hello client.example.com
  ```

---

### **Step 3: Specify the Sender**
- The client sends the **MAIL FROM** command:
  ```
  MAIL FROM:<alice@example.com>
  ```
- Server replies:
  ```
  250 OK
  ```

---

### **Step 4: Specify the Recipient**
- The client sends the **RCPT TO** command:
  ```
  RCPT TO:<bob@example.com>
  ```
- Server replies:
  ```
  250 OK
  ```

(You can repeat this command for **multiple recipients**.)

---

### **Step 5: Begin Message Content**
- The client tells the server it's ready to send the actual message with the **DATA** command:
  ```
  DATA
  ```
- Server replies:
  ```
  354 Start mail input; end with <CRLF>.<CRLF>
  ```

---

### **Step 6: Send Email Headers**
- The client sends email **headers** (one per line):
  ```
  From: alice@example.com
  To: bob@example.com
  Subject: Hello from SMTP!
  Date: Tue, 16 Apr 2025 15:30:00 +0530
  ```

(These are just plain text lines that describe the metadata.)

---

### **Step 7: Send Email Body**
- After a blank line, the **body of the email** is written:
  ```
  Hi Bob,

  This is a test message sent using SMTP.

  Regards,
  Alice
  ```

---

### **Step 8: End the Message**
- The client ends the message by sending a line with just a **single period (`.`)**:
  ```
  .
  ```
- Server replies:
  ```
  250 OK: Message accepted
  ```

---

### **Step 9: Close the Connection**
- The client ends the session with:
  ```
  QUIT
  ```
- Server replies:
  ```
  221 Bye
  ```

---

### 📌 Summary:
| Section         | Content                              |
|----------------|---------------------------------------|
| **Headers**     | From, To, Subject, Date, etc.         |
| **Body**        | Main message text                     |
| **End of Data** | A line with a single `.`              |

> **Note:** SMTP does not itself display or store the email—it just **transfers it** to the recipient's mail server.

---

