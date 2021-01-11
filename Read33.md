# Authentication & Production Server

## What is JSON Web Token

JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

## When should you use JSON Web Tokens

Here are some scenarios where JSON Web Tokens are useful:

- **Authorization:** This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token.

- **Information Exchange:** JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are.

## What is the JSON Web Token structure

In its compact form, JSON Web Tokens consist of three parts separated by dots (.), which are:

- Header
- Payload
- Signature

Therefore, a JWT typically looks like the following.

`xxxxx.yyyyy.zzzzz`

### Header

The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.

For example:

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional data.

There are three types of claims:

- **Registered claims:** These are a set of predefined claims which are not mandatory but recommended, to provide a set of useful, interoperable claims.

- **Public claims:** These can be defined at will by those using JWTs. But to avoid collisions they should be defined in the IANA JSON Web Token Registry or be defined as a URI that contains a collision resistant namespace.

- **Private claims:** These are the custom claims created to share information between parties that agree on using them and are neither registered or public claims.

An example payload could be:

```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

### Signature

To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For example if you want to use the HMAC SHA256 algorithm, the signature will be created in the following way:

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

The signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is.

## How do JSON Web Tokens work

In authentication, when the user successfully logs in using their credentials, a JSON Web Token will be returned. Since tokens are credentials, great care must be taken to prevent security issues. In general, you should not keep tokens longer than required.

If the token is sent in the Authorization header, Cross-Origin Resource Sharing (CORS) won't be an issue as it doesn't use cookies.

The following diagram shows how a JWT is obtained and used to access APIs or resources:


![image](assets/Read33-1.png)

- The application or client requests authorization to the authorization server. This is performed through one of the different authorization flows.
- When the authorization is granted, the authorization server returns an access token to the application.
- The application uses the access token to access a protected resource (like an API).

## Why should we use JSON Web Tokens

As JSON is less verbose than XML, when it is encoded its size is also smaller, making JWT more compact than SAML. This makes JWT a good choice to be passed in HTML and HTTP environments.

Security-wise, SWT can only be symmetrically signed by a shared secret using the HMAC algorithm. However, JWT and SAML tokens can use a public/private key pair in the form of a X.509 certificate for signing.

Regarding usage, JWT is used at Internet scale. This highlights the ease of client-side processing of the JSON Web token on multiple platforms, especially mobile.

# How to Use JWT Authentication with Django REST Framework

JWT stand for JSON Web Token and it is an authentication strategy used by client/server applications where the client is a Web application using JavaScript and some frontend framework like Angular, React or VueJS.

## How JWT Works

The JWT is just an authorization token that should be included in all requests:

```
curl http://127.0.0.1:8000/hello/ -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTQzODI4NDMxLCJqdGkiOiI3ZjU5OTdiNzE1MGQ0NjU3OWRjMmI0OTE2NzA5N2U3YiIsInVzZXJfaWQiOjF9.Ju70kdcaHKn1Qaz8H42zrOYk0Jx9kIckTn9Xx7vhikY'
```

The JWT is acquired by exchanging an username + password for an access token and an refresh token.

The access token is usually short-lived (expires in 5 min or so, can be customized though).

The refresh token lives a little bit longer (expires in 24 hours, also customizable). It is comparable to an authentication session. After it expires, you need a full login with username + password again.

Why is that?

It’s a security feature and also it’s because the JWT holds a little bit more information.

This information is encoded using Base64. If we decode, we will see something like this:

- **header**

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

- **payload**

```
{
  "token_type": "access",
  "exp": 1543828431,
  "jti": "7f5997b7150d46579dc2b49167097e7b",
  "user_id": 1
}
```

- **signature**

The signature is issued by the JWT backend, using the header base64 + payload base64 + SECRET_KEY. Upon each request this signature is verified. If any information in the header or in the payload was changed by the client it will invalidate the signature. The only way of checking and validating the signature is by using your application’s SECRET_KEY.

## Usage

We use HTTPie to consume the API endpoints via the terminal. But you can also use cURL (readily available in many OS) to try things out locally.

Obtain Token

First step is to authenticate and obtain the token. The endpoint is /api/token/ and it only accepts POST requests.

```
http post http://127.0.0.1:8000/api/token/ username=vitor password=123
```

So basically your response body is the two tokens:

```
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTQ1MjI0MjU5LCJqdGkiOiIyYmQ1NjI3MmIzYjI0YjNmOGI1MjJlNThjMzdjMTdlMSIsInVzZXJfaWQiOjF9.D92tTuVi_YcNkJtiLGHtcn6tBcxLCBxz9FKD3qzhUg8",
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTU0NTMxMDM1OSwianRpIjoiMjk2ZDc1ZDA3Nzc2NDE0ZjkxYjhiOTY4MzI4NGRmOTUiLCJ1c2VyX2lkIjoxfQ.rA-mnGRg71NEW_ga0sJoaMODS5ABjE5HnxJDb0F8xAo"
}
```

After that you are going to store both the access token and the refresh token on the client side, usually in the localStorage.

In order to access the protected views on the backend (i.e., the API endpoints that require authentication), you should include the access token in the header of all requests, like this:

```
http http://127.0.0.1:8000/hello/ "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTQ1MjI0MjAwLCJqdGkiOiJlMGQxZDY2MjE5ODc0ZTY3OWY0NjM0ZWU2NTQ2YTIwMCIsInVzZXJfaWQiOjF9.9eHat3CvRQYnb5EdcgYFzUyMobXzxlAVh_IAgqyvzCE"
```

Refresh Token

To get a new access token, you should use the refresh token endpoint /api/token/refresh/ posting the refresh token:

```
http post http://127.0.0.1:8000/api/token/refresh/ refresh=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTU0NTMwODIyMiwianRpIjoiNzAyOGFlNjc0ZTdjNDZlMDlmMzUwYjg3MjU1NGUxODQiLCJ1c2VyX2lkIjoxfQ
```

What’s The Point of The Refresh Token

At first glance the refresh token may look pointless, but in fact it is necessary to make sure the user still have the correct permissions. If your access token have a long expire time, it may take longer to update the information associated with the token. That’s because the authentication check is done by cryptographic means, instead of querying the database and verifying the data. So some information is sort of cached.
