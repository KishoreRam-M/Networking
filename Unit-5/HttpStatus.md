## 📦 What Are HTTP Status Codes?
When a server responds to a request, it sends back a **status code** — a 3-digit number that tells the client what happened.

These codes are grouped by their **first digit (1-5)**:

---

## 🔢 The Five Types of HTTP Status Codes:

| Code Range | Type                    | Meaning                             |
|------------|-------------------------|-------------------------------------|
| 1xx        | ✅ Informational         | "Got your request, processing…"     |
| 2xx        | 🟢 Success               | "Everything went well!"             |
| 3xx        | 🟡 Redirection           | "Go somewhere else for this."       |
| 4xx        | 🔴 Client Error          | "You messed up the request."        |
| 5xx        | 🔥 Server Error          | "Server messed up internally."      |

---

### 1️⃣ **1xx – Informational**
> 👋 Just letting you know that we’re working on it.

- **100 Continue** – Server got the headers, client should continue with the body.
- **101 Switching Protocols** – Server is switching to a different protocol (like from HTTP to WebSocket).

👉 You don’t usually see these in browsers.

---

### 2️⃣ **2xx – Success**
> ✅ Everything worked as expected.

- **200 OK** – Standard success message.
- **201 Created** – Something new was created (like a new user).
- **204 No Content** – Request was successful but there's no response body.

---

### 3️⃣ **3xx – Redirection**
> 🔄 The resource has moved. Go look over there.

- **301 Moved Permanently** – URL has permanently changed.
- **302 Found (Temporary Redirect)** – Resource is temporarily somewhere else.
- **304 Not Modified** – Cached version is still valid; no need to resend it.

---

### 4️⃣ **4xx – Client Errors**
> ❌ The problem is on **your side** (the browser/client).

- **400 Bad Request** – Request is invalid or malformed.
- **401 Unauthorized** – You need to log in (missing or invalid auth).
- **403 Forbidden** – You're not allowed, even if logged in.
- **404 Not Found** – The resource doesn’t exist.
- **429 Too Many Requests** – You’re sending too many requests (rate-limited).

---

### 5️⃣ **5xx – Server Errors**
> 🧨 The server tried, but failed. The issue is **on the server’s side**.

- **500 Internal Server Error** – Generic server error (something broke).
- **502 Bad Gateway** – Server got an invalid response from another server.
- **503 Service Unavailable** – Server is down or too busy.
- **504 Gateway Timeout** – Server took too long to respond.

---

### 🎯 Quick Visual Summary:

```plaintext
1xx ➡️ Processing  
2xx ✅ Success  
3xx 🔁 Redirect  
4xx 🚫 Client Error  
5xx 💥 Server Error
```
