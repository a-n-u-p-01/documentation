Password encoding is the process of securely transforming a user’s password before storing it in the database. Instead of saving the original password, a hashed version is stored so that even if the database is compromised, attackers cannot easily obtain user credentials.

Spring Security provides the **PasswordEncoder** interface for this purpose, and **BCrypt** is the recommended implementation.

---

## What is BCrypt?

BCrypt is a password hashing algorithm designed specifically for secure password storage.

Key characteristics:

- One-way hashing (cannot be reversed)
    
- Automatically generates a salt
    
- Computationally slow to resist brute-force attacks
    
- Adaptive strength (can increase difficulty over time)
    

Because of these properties, BCrypt is widely accepted as a secure standard for backend systems.

---

## Why Password Encoding is Required

Storing plain-text passwords is a critical security failure.

Example (incorrect):

```
password: mypassword123
```

If leaked, the account is instantly compromised.

Correct approach:

```
password: $2a$10$XJH72kjsd8s7d6...
```

The stored value is a hash, not the original password.

---

## How BCrypt Works

### 1. Hashing

The original password is transformed into a fixed-length string.

```
mypassword123 → hashed value
```

The original password cannot be reconstructed from the hash.

---

### 2. Salting

BCrypt automatically adds random data (salt) before hashing.

This ensures:

- Same password never produces the same hash.
    
- Rainbow table attacks become ineffective.
    

You do not manage the salt manually.

---

### 3. Adaptive Cost (Strength Factor)

BCrypt includes a configurable work factor that determines how computationally expensive hashing is.

Higher strength:

- More secure
    
- Slower hashing
    

Lower strength:

- Faster
    
- Less resistant to attacks
    

Default strength (10) is suitable for most applications.

---

## Encoding vs Encryption

**Encoding / Hashing**

- One-way
    
- Cannot be decrypted
    
- Used for passwords
    

**Encryption**

- Two-way
    
- Can be decrypted with a key
    
- Used for sensitive data like financial information
    

Passwords must always be hashed, never encrypted.

---

## How Spring Security Uses BCrypt

### During Registration

1. User provides a password.
    
2. Password is encoded using BCrypt.
    
3. Hash is stored in the database.
    

### During Login

1. User enters password.
    
2. Spring hashes the entered password.
    
3. Compares it with the stored hash.
    
4. If they match → authentication succeeds.
    

You never manually compare passwords.

---

## Important Practices

- Always encode passwords before saving users.
    
- Never log passwords.
    
- Never send passwords back in API responses.
    
- Do not attempt to decode hashes.
    
- Keep password encoding centralized in configuration.
    

---

## When to Use BCrypt

Use BCrypt whenever your application stores user credentials, including:

- database authentication
    
- REST APIs
    
- microservices
    
- enterprise systems
    

It should be the default choice unless a stronger adaptive algorithm is specifically required.

---

## Summary

BCrypt securely hashes passwords using salting and adaptive complexity, ensuring that stored credentials remain protected even if the database is exposed. It is the recommended password encoder for Spring Security applications.