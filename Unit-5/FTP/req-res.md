## 📡 **FTP Commands & Responses (Control Channel)**

When your computer talks to an FTP server, it uses:
- **Commands** to tell the server what to do
- **Responses** to understand what the server says back

This all happens over the **Control Channel** — like texting back and forth with the server.

---

### ✉️ **FTP Commands** = What **You** send to the server  
These are plain-text instructions. Think of it like sending:
> "Hey server, do this please!"

| Command | Meaning                            |
|---------|-------------------------------------|
| `USER`  | Login with a username               |
| `PASS`  | Login with a password               |
| `CWD`   | Change directory on the server      |
| `LIST`  | Show list of files/folders          |
| `RETR`  | Download a file                     |
| `STOR`  | Upload a file to the server         |

🧠 **Example Conversation:**
```bash
USER kishore
PASS secret123
CWD /documents
LIST
RETR resume.pdf
```

---

### 📲 **FTP Responses** = What the **Server** sends back  
These are 3-digit codes + messages that tell you what’s happening.

---

### ✅ Common FTP Response Codes:

| Code | Meaning                             | What’s Happening                          |
|------|-------------------------------------|-------------------------------------------|
| 220  | Service ready                       | "Hi, I'm the FTP server. Ready to talk!"  |
| 331  | Username okay, need password        | "Username accepted, now give password."   |
| 230  | User logged in                      | "Login successful!"                       |
| 150  | File status okay, starting transfer | "Starting to send the file..."            |
| 226  | Closing data connection             | "File sent successfully!"                 |
| 530  | Not logged in / login failed        | "Wrong username or password."             |

---

### 💬 Real-Life Example (like chatting):

```bash
Client: USER kishore
Server: 331 Username okay, need password.

Client: PASS secret123
Server: 230 User logged in, proceed.

Client: LIST
Server: 150 Opening data connection for directory listing.
Server: 226 Directory send OK.

Client: RETR notes.pdf
Server: 150 Opening data connection for notes.pdf
Server: 226 Transfer complete.
```

---
