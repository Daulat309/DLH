# Authentication & Authorization — Revision Notes

## 1. Authentication vs Authorization

### Authentication

**Authentication = "Who are you?"**

It verifies the identity of a user.

Examples:

-  Username + Password 
-  OTP 
-  Biometrics 
-  Login using Google/Facebook 

### Authorization

**Authorization = "What can you do?"**

It determines what an authenticated user is allowed to access or perform.

Examples:

-  A normal user can view their profile. 
-  An admin can view, modify, or delete users. 
-  A student can access course material but cannot modify it. 

### Easy way to remember

```
Authentication → Who are you?
Authorization → What can you do?
```

### Comparison

| Authentication | Authorization |
| --- | --- |
| Verifies user identity | Determines permissions |
| Happens first | Happens after authentication |
| Answers "Who are you?" | Answers "What can you do?" |
| Uses credentials/tokens | Uses roles, permissions, access rules |

---

## 2. Basic Authentication

Basic Authentication is a simple authentication mechanism where the client sends a username and password to the application/server.

### Flow

```
Client                         App
  |                             |
  | -------- Register --------> |
  |                             |
  | <---- Username & Password -- |
  |                             |
  | ---- Username & Password --> |
  |                             |
  | <--------- Response -------- |
  |                             |
  | ---- Username & Password --> |
  |                             |
  | <--------- Response -------- |
```

### Working

1. User registers with the application. 
2. The user receives/sets a username and password. 
3. During login, the client sends the username and password to the application. 
4. The application verifies the credentials. 
5. The application sends a response. 

### Main idea

```
Client → Username + Password → Server
Client ← Response            ← Server
```

### Important point

Basic authentication requires the client to provide credentials repeatedly when authentication is required.

---

## 3. Token-Based Authentication

In Token-Based Authentication, the user initially logs in using their credentials, and the application provides a token.

The client then uses the token for subsequent requests instead of repeatedly sending the username and password.

### Flow

```
Client                         App
  |                             |
  | -------- Register --------> |
  |                             |
  | <---- Username & Password -- |
  |                             |
  | -- Login (Username + PW) -> |
  |                             |
  | <---------- Token ---------- |
  |                             |
  | ----------- Token ---------> |
  |                             |
  | <--------- Response -------- |
  |                             |
  | ----------- Token ---------> |
  |                             |
  | <--------- Response -------- |
```

### Working

1. User registers. 
2. User logs in using username + password. 
3. The application verifies the credentials. 
4. The application generates/returns a token. 
5. The client stores the token. 
6. For subsequent requests, the client sends the token. 
7. The application validates the token and returns the requested response. 

### Token flow

```
Username + Password
        ↓
      Login
        ↓
   Authentication
        ↓
      Token
        ↓
Token → Server → Response
Token → Server → Response
```

---

## 4. Token Expiration

A token generally has a limited lifetime.

Token will expire after a specified time.

For example:

```
Login
  ↓
Token generated
  ↓
Token valid for a certain period
  ↓
Token expires
  ↓
User must authenticate again / obtain a new token
```

### Why use expiration?

Token expiration limits the period for which a token can be used and improves security.

**Example:** Banking websites/apps may require users to authenticate again after a period of inactivity or when a session expires.

---

## 5. Basic Authentication vs Token-Based Authentication

| Feature | Basic Authentication | Token-Based Authentication |
| --- | --- | --- |
| Initial credentials | Username + Password | Username + Password |
| Subsequent requests | Credentials are sent | Token is sent |
| Token | Not the main mechanism | Main authentication credential |
| Expiration | Not based on a token | Token can expire |
| Common idea | Authenticate using credentials | Authenticate once and use token |

### Remember

```
Basic Authentication
→ Username + Password repeatedly

Token-Based Authentication
→ Login with Username + Password
→ Receive Token
→ Use Token for later requests
```

---

## 6. OAuth

OAuth is an authorization framework that allows an application to obtain limited access to a user's resources through another service without requiring the application to receive the user's password.

The references show OAuth with services such as:

-  Google 
-  Instagram 
-  LinkedIn 
-  Facebook 
-  Amazon 
-  Pinterest 

### Main idea

Instead of giving an application your password for another service, you can authorize access through that service.

For example:

```
User
  ↓
Google
  ↓
Other Application
```

The external service handles the user's authentication, and the application receives an appropriate authorization credential/token.

---

## 7. OAuth Example — "Login with Google"

A common example is:

**Login with Google**

Suppose an application wants to let a user access it using their Google account.

### Simplified flow

```
User → Application

Application → Google
                ↓
        User authenticates
                ↓
        User grants permission
                ↓
Application ← Token/Authorization
                ↓
Application provides access
```

The important idea is:

```
User does NOT give the application
their Google password.
```

Instead:

```
User → Google → Permission → Application
```

---

## 8. Authentication and OAuth

OAuth is primarily an authorization framework, although it is commonly used in login systems together with an identity layer such as OpenID Connect.

For revision purposes:

```
Authentication
→ Establishes identity

Authorization
→ Grants access/permissions

OAuth
→ Allows delegated authorization
   between applications/services
```

### Example

If an application asks:

> "Allow this application to access your Google account information?"

The user can grant or deny that permission.

---

## 9. Quick Revision

### Authentication

**Question:**

> Who are you?

**Purpose:** Verify identity.

Examples:

-  Username + password 
-  OTP 
-  Biometrics 
-  Login through an identity provider 

### Authorization

**Question:**

> What can you do?

**Purpose:** Determine access and permissions.

Examples:

-  User can view profile. 
-  Admin can delete users. 
-  Employee can access specific resources. 

### Basic Authentication

```
Username + Password
        ↓
      Server
        ↓
     Response
```

### Token-Based Authentication

```
Username + Password
        ↓
       Login
        ↓
      Token
        ↓
Token → Server
Token → Server
Token → Server
```

Token expires after a specified time.

### OAuth

```
User
 ↓
Google / Facebook / Other Provider
 ↓
Grant Permission
 ↓
Application receives authorization
 ↓
Access granted
```

---

## ⭐ Exam Shortcut

```
Authentication = Identity
Authorization = Permission
Basic Auth = Username + Password
Token Auth = Login → Token → Use Token
OAuth = Delegated authorization without sharing the external service's password
```
