# What is JWT?

JWT (JSON Web Token) is an open standard for securely transmitting information between parties as a JSON object. It is often used to authenticate users in a web application and transmit information such as user identities and claims.

## **How does JWT work?**

![A Primer on JSON Web Tokens | PSPDFKit](https://pspdfkit.com/assets/images/blog/2021/a-primer-on-json-web-tokens/authorization-flow-bd7f07f4.png align="center")

A JWT consists of three parts: a header, a payload, and a signature. The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA. The payload contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.

The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

## **Example of a JWT**

Here is an example of a JWT:

```csharp
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

This JWT consists of three strings, separated by dots (`.`). The first string is the base64-encoded header, the second is the base64-encoded payload, and the third is the signature.

## JWT Parts

As mentioned earlier, a JWT consists of three parts: a header, a payload, and a signature.

### **Header**

The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used.

An example of a JWT header could be:

```csharp
{
  "alg": "HS256",
  "typ": "JWT"
}
```

In this example, the signing algorithm used is HMAC SHA256.

### **Payload**

The payload contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.

An example of a JWT payload could be:

```csharp
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

In this example, the `sub` claim is the subject of the token and identifies the user. The `name` claim is the user's name, and the `iat` claim is the time the JWT was issued.

### **Signature**

The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way. The signature is created by taking the encoded header, the encoded payload, and a secret, and then signing them with the algorithm specified in the header.

Here is an example of how the signature is created using the HMAC SHA256 algorithm:

```csharp
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

The resulting signature is then appended to the end of the encoded header and encoded payload, separated by a dot (`.`). The resulting string is the complete JWT.

## **Using JWT for Authentication**

One common use of JWT is for authenticating users. When a user logs in to a web application, the server creates a JWT and sends it back to the user. The user then stores the JWT in a cookie or local storage and sends it back to the server with each subsequent request. The server can then verify the JWT to authenticate the user.

Here is an example of a payload for a JWT that contains information about a user:

```csharp
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

The `sub` claim is the subject of the token and identifies the user. The `name` claim is the user's name, and the `iat` claim is the time the JWT was issued.

## **Conclusion**

JWT is a widely used standard for transmitting information securely between parties, especially in web applications. It allows users to be authenticated and transmit information such as user identities and claims in a secure way.