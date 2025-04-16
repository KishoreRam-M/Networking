### 🗂️ **FTP Process Flow:**
When you connect to an FTP server to transfer files, there’s a sequence of steps that happen to make everything work. Here’s a simplified version:

---

### 1️⃣ **Connection Establishment:**
- The **client** (your computer) **opens a connection** to the server’s **port 21**. Think of this as ringing the bell to the server.
- **Server responds** with a welcome message like **“220 Service ready”**, saying, “Hey, I’m ready to talk!”

---

### 2️⃣ **Authentication (Login Process):**
- **Client sends** the **USER** command with the **username**:  
  For example, `USER john`  
  The server replies: **“331 User name okay, need password”** — basically saying, “I know you, but I need your password.”
  
- **Client sends** the **PASS** command with the **password**:  
  For example, `PASS mySecret123`

- The server confirms: **“230 User logged in, proceed”** — this means, “You’re in! Now, let’s do some file stuff.”

---

### 3️⃣ **Issuing File Commands (Interacting with Files):**
Once you're logged in, you can start working with files:
- **LIST**: Ask the server for a list of files in a directory (like opening a folder to see its contents).
  Example: `LIST`
  
- **CWD**: Change directory to move around the server's file system.
  Example: `CWD /documents` to go to the **/documents** folder.

- **Retrieving files**: Use **RETR** to download files.
  Example: `RETR file1.txt` to download a file.

- **Uploading files**: Use **STOR** to upload files to the server.
  Example: `STOR file2.txt` to upload a file.

---

### 4️⃣ **Data Transfer (Sending or Receiving Files):**
- When you're downloading or uploading files, a **separate data connection** is created.
- After the file transfer, this data connection **closes**, but the control connection (used for commands) stays open, allowing you to continue interacting with the server.

---

### 5️⃣ **Termination (Ending the Session):**
- Once you're done, you tell the server to end the session:
  Example: **QUIT** command to close the connection.
  The server responds, and the **control connection is closed**.

---

### 🎯 **Summary**:
- **Step 1**: Connect to server (port 21).
- **Step 2**: Login (send username & password).
- **Step 3**: Send file commands (like `LIST`, `RETR`, `STOR`).
- **Step 4**: Transfer data over a separate channel.
- **Step 5**: End session with **QUIT**.
