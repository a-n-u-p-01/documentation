UserDetailsService is a core Spring Security interface used to load user data during authentication. When a user tries to log in, Spring Security calls this service to retrieve the user from the database and provide the necessary details for verification.

It acts as the bridge between your application database and Spring Security.

---

## Purpose

The primary responsibility of UserDetailsService is:

> Fetch the user by a unique identifier and return a UserDetails object.

Method:

```
UserDetails loadUserByUsername(String username)
```

The “username” can be email, phone number, or any unique field.

---

## Role in Authentication Flow

```
Login Request
    ↓
AuthenticationManager
    ↓
DaoAuthenticationProvider
    ↓
UserDetailsService
    ↓
Database
    ↓
Password Check
```

If the user is not found, authentication fails immediately.

---

## What Your Implementation Must Do

### 1. Retrieve User from Database

Use a repository to find the user.

If not found → throw `UsernameNotFoundException`.

---

### 2. Return UserDetails

Spring Security requires a UserDetails object containing:

- username
    
- encoded password
    
- authorities (roles)
    
- account status
    

Spring uses this data to validate credentials and determine access.

---

## UserDetails vs User Entity

Two common approaches:

### Implement UserDetails in Entity

Your User class directly implements UserDetails.

**Pros:**

- Simple
    
- Less code
    

**Cons:**

- Couples security with database model
    

---

### Create a Custom UserDetails Class (Recommended)

Wrap the User entity inside a class that implements UserDetails.

**Pros:**

- Cleaner architecture
    
- Better separation of concerns
    
- Easier to modify later
    

Preferred in production systems.

---

## Important Points

- Password must already be encoded (e.g., BCrypt).
    
- Username/email should be unique.
    
- Always map roles to authorities.
    
- Do not return null — throw an exception if user is missing.
    

---

## When It Is Used

UserDetailsService is required when authentication depends on stored users, such as:

- database authentication
    
- JWT-based systems
    
- role-based access control
    

It is not needed for basic in-memory setups.

---