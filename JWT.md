**JWT** stands for **JSON Web Token**. It is an open standard (RFC 7519) used to securely transmit information between two parties
# 1. The Old Way vs The JWT Way
1. Tradition Session Authentication (Stateful)
	- You login with username and password
	- The server creates a "Session ID," saves it in its database/memory, and sends it back to your browser as a cookie.
	- On every future request, the browser sends the Session ID. The server must stop what it's doing, look up that ID in its database, and verify who you are.
	- **The Problem:** If you have 5 different servers running your app, they all need access to a shared database or session store (like Redis) to verify your ID. This makes scaling difficult.
2. Jwt Authentication (Stateless)
	- You log in with your username and password.
	- The server verifies your credentials and creates a JWT. Inside this token, the server encodes your user information (e.g., `userId: 123`, `role: "ADMIN"`).
	- The server **signs** this token using a secret password known only to the server, then sends the JWT back to your frontend app.
	- Your frontend saves the JWT (usually in `localStorage` or a cookie) and attaches it to the header of every future request.
	- **The Benefit:** When the server receives the token, it doesn't look anything up in a database. It simply applies a mathematical formula using its secret password to verify that the token is authentic. If the signature matches, the server trusts the data inside it completely.
# 2. The structure of a JWT
## 2.1. The Header
The header typically consists of two parts: the type of the token (which is JWT) and the signing algorithm being used, such as HMAC SHA256 or RSA.
```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```
## 2.2. The Payload
This contains the actual data you want to share, known as "Claims." This is where you put user information, application roles, and token expiration times.
```JSON
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true,
  "exp": 1716941200
}
```
## 2.3. The Signature
To create the signature part, the server takes the encoded header, the encoded payload, a secret password of your choosing, and runs them through the algorithm specified in the header.
```JSON
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  your-256-bit-secret-key
)
```
The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't tampered with along the way.