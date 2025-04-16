### 🔹 Types of HTTP Headers:

1. **Request Headers** – Sent by the client (browser, app) to provide information about the request.
2. **Response Headers** – Sent by the server to give info about the response.
3. **General Headers** – Can apply to both request and response (e.g., `Cache-Control`).
4. **Entity Headers** – Give details about the body of the message (e.g., `Content-Type`, `Content-Length`).

---

### 📨 Example of Request Headers:
```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Authorization: Bearer abc123
```

- `Host`: Which domain you're accessing.
- `User-Agent`: Info about the client/browser.
- `Accept`: What content types the client supports.
- `Authorization`: Used for login/auth tokens.

---

### 📤 Example of Response Headers:
```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1024
Set-Cookie: sessionId=abc123; HttpOnly
Cache-Control: no-cache
```

- `Content-Type`: Type of data being sent (HTML, JSON, etc.).
- `Set-Cookie`: Server sets a cookie on your browser.
- `Cache-Control`: How the response should be cached.

---

### ✅ Why Headers Matter:
- They **control behavior** (like caching, authentication, compression).
- Help in **negotiating formats** (e.g., language, content type).
- Enable **security features** (like CORS, CSP).
- Carry **authentication tokens, cookies, content length**, etc.

---
