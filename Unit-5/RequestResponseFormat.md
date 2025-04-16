## 📤 HTTP Request Message Format

Sent **from client to server** when you want to ask for something (like a web page or API data).

```http
1️⃣ Request Line
2️⃣ Headers
3️⃣ (Optional) Body
```

### ✅ Example:

```http
GET /products HTTP/1.1
Host: example.com
User-Agent: Chrome/123
Accept: application/json

(no body for GET request)
```

#### 🔹 Parts:
- **Request Line** – Method + Resource + HTTP Version (e.g., `GET /home HTTP/1.1`)
- **Headers** – Key-value pairs giving extra info (e.g., what format you accept)
- **Body (Optional)** – Data you send (used with POST, PUT, etc.)

---

## 📥 HTTP Response Message Format

Sent **from server to client** as a reply.

```http
1️⃣ Status Line
2️⃣ Headers
3️⃣ (Optional) Body
```

### ✅ Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 27

{
  "product": "Laptop"
}
```

#### 🔹 Parts:
- **Status Line** – HTTP Version + Status Code + Message (e.g., `HTTP/1.1 200 OK`)
- **Headers** – Info about the response (e.g., type of data, caching)
- **Body (Optional)** – Actual content (HTML, JSON, image, etc.)

---

### 🧠 Quick Comparison:

| HTTP Request               | HTTP Response               |
|---------------------------|-----------------------------|
| Method + URI + Version    | Version + Status Code       |
| Headers                   | Headers                     |
| Optional Body (POST data) | Optional Body (returned data)|
