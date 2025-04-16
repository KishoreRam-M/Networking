## 📧 **SMTP Message Format**

SMTP (Simple Mail Transfer Protocol) uses a **text-based message format**. That means everything — commands, responses, headers, and body — is in **readable plain text**.

### ✅ SMTP Email Structure:
When an email is sent via SMTP, it has **two major parts**:

---

### 1️⃣ **Email Headers** (Meta-information)
These go **before the message body** and describe key details about the email.

Example headers:
```
From: sender@example.com
To: receiver@example.com
Subject: Meeting Reminder
Date: Wed, 17 Apr 2025 12:00:00 GMT
```

---

### 2️⃣ **Email Body** (Main content)
This is the **actual message** — what you wrote.

Example:
```
Hi there,

Just a reminder about our meeting tomorrow at 10 AM.

Thanks!
```

---

### 🔚 End of Message:
The message ends with:
```
.
```
A **single line with a dot (`.`)** indicates to the server that the message is finished.

---

## 🛠️ **Common SMTP Commands**

SMTP uses specific commands to manage the conversation between the email client and the server. Each command is **a single line of text**, followed by the required info.

| Command     | Meaning                                         |
|-------------|--------------------------------------------------|
| `HELO`      | Starts the session; client introduces itself     |
| `EHLO`      | Extended version of HELO (supports extra features) |
| `MAIL FROM:`| Specifies the sender's email address             |
| `RCPT TO:`  | Specifies the recipient's email address          |
| `DATA`      | Starts the actual message (headers + body)       |
| `RSET`      | Resets the session (clears sender/recipient info)|
| `VRFY`      | Verifies if an email address exists              |
| `NOOP`      | No operation; just checks connection             |
| `QUIT`      | Ends the session and closes the connection       |

---

### 🧪 Example SMTP Conversation

```
Client: HELO mydomain.com
Server: 250 Hello mydomain.com

Client: MAIL FROM:<me@example.com>
Server: 250 OK

Client: RCPT TO:<you@example.com>
Server: 250 OK

Client: DATA
Server: 354 End data with <CRLF>.<CRLF>

Client:
From: me@example.com
To: you@example.com
Subject: Hello

This is the message.

.
Server: 250 OK, message accepted

Client: QUIT
Server: 221 Bye
```

---
