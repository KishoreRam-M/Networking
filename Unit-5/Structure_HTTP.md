# 🧱 Structure of an HTTP Request

An HTTP request is made up of **three main parts**:

```
1️⃣ Request Line  
2️⃣ Headers  
3️⃣ (Optional) Body  
```

Let’s break them down:

---

## 1️⃣ Request Line

The **first line** of the HTTP request. It tells the server **what** you want to do.

### 🔹 Format:
```
<HTTP Method> <Request URI> <HTTP Version>
```

### 🔹 Example:
```
GET /products/123 HTTP/1.1
```

- `GET` → The HTTP method (action)  
- `/products/123` → The resource you're asking for  
- `HTTP/1.1` → The version of HTTP protocol being used  

---

## 2️⃣ Headers

Headers are **key-value pairs** that provide additional information about the request, like browser type, accepted formats, or authentication.

Example headers:
```
Host: example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer abc123
```

> ✅ Check the previous section for a detailed breakdown of HTTP headers.

---

## 3️⃣ (Optional) Body

The **request body** is only included in methods like `POST`, `PUT`, or `PATCH`.

### 🔹 Used to send:
- Form data
- JSON objects
- Files
- Any other data

### 🔹 Example (POST request):
```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 48

{
  "username": "john",
  "password": "secret"
}
```

- The JSON is the **request body**.
- It's optional because methods like `GET` usually don't include a body.

---

## 🔁 Quick Recap

| Part             | Purpose                                           |
|------------------|---------------------------------------------------|
| **Request Line** | What action to perform and on which resource      |
| **Headers**      | Extra info (format, auth, browser, etc.)          |
| **Body** (Optional) | Actual data being sent to the server             |

---
