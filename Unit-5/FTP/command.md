### 🔑 1️⃣ `USER`  
➡️ **Meaning:** "This is my username."

Used to tell the FTP server who you are.

🧠 **Example:**  
```
USER kishore123
```

---

### 🔐 2️⃣ `PASS`  
➡️ **Meaning:** "Here’s my password."

Sent after the `USER` command to log in.

🧠 **Example:**  
```
PASS mySecretPassword
```

---

### 📂 3️⃣ `LIST`  
➡️ **Meaning:** "Show me the files in this folder."

Used to get a list of files and directories on the server (like `ls` in Linux).

🧠 **Example:**
```
LIST
```

Server replies with:
```
file1.txt
file2.jpg
documents/
```

---

### 📥 4️⃣ `RETR`  
➡️ **Meaning:** "I want to download this file."

Used to **retrieve** a file from the server.

🧠 **Example:**  
```
RETR notes.pdf
```

This command triggers the **data connection**, and the file `notes.pdf` is sent to you.

---

### 🔁 Summary Table:

| Command | Meaning                        | Action                          |
|---------|--------------------------------|----------------------------------|
| `USER`  | Provide your username          | Start login process              |
| `PASS`  | Provide your password          | Complete login                   |
| `LIST`  | List files/directories         | See what's on the server         |
| `RETR`  | Retrieve a file                | Download from the server         |
