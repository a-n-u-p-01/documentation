# Goal

Build Spring Security authentication using:

✅ Spring Boot  
✅ Spring Security  
✅ JPA  
✅ MySQL/Postgres (or H2)  
✅ BCrypt  
✅ UserDetailsService

No JWT yet — first master core authentication.

---

# Step 1 — Add Dependencies

If using Maven:

```xml
<dependencies>

    <!-- Spring Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Security -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Database -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
    </dependency>

    <!-- Optional but recommended -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>

</dependencies>
```

Start with **H2** if learning — zero setup.

---

# Step 2 — Create User Entity

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true)
    private String email;

    private String password;

    private String role; // ROLE_USER, ROLE_ADMIN
}
```

Keep it simple.

Do NOT overengineer.

---

# Step 3 — Create Repository

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email);
}
```

Spring Security will use this indirectly.

---

# Step 4 — Create Custom UserDetails

**Do NOT implement UserDetails in the entity.**  
Cleaner architecture = easier scaling later.

```java
public class CustomUserDetails implements UserDetails {

    private final User user;

    public CustomUserDetails(User user){
        this.user = user;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return List.of(
            new SimpleGrantedAuthority(user.getRole())
        );
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getEmail();
    }

    @Override public boolean isAccountNonExpired(){ return true; }
    @Override public boolean isAccountNonLocked(){ return true; }
    @Override public boolean isCredentialsNonExpired(){ return true; }
    @Override public boolean isEnabled(){ return true; }
}
```

Now Spring understands your user.

---

# Step 5 — Implement UserDetailsService

🔥 VERY IMPORTANT CLASS.

```java
@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {

    private final UserRepository repo;

    @Override
    public UserDetails loadUserByUsername(String email)
            throws UsernameNotFoundException {

        User user = repo.findByEmail(email)
                .orElseThrow(() ->
                        new UsernameNotFoundException("User not found"));

        return new CustomUserDetails(user);
    }
}
```

This is the bridge:

DB → Spring Security.

---

# Step 6 — Configure BCrypt

```java
@Configuration
public class SecurityBeans {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
}
```

Never store plain passwords.

---

# Step 7 — Security Configuration (CRITICAL)

Without this → nothing is protected.

```java
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final CustomUserDetailsService service;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

        http
            .csrf(csrf -> csrf.disable()) // disable for testing APIs
            .authorizeHttpRequests(auth -> auth
                    .requestMatchers("/auth/**").permitAll()
                    .anyRequest().authenticated()
            )
            .userDetailsService(service)
            .httpBasic(); // simplest login mechanism

        return http.build();
    }
}
```

We use **Basic Auth** just to verify DB authentication works.

JWT comes later.

Walk before running.

---

# Step 8 — Create Registration API

Because users must exist in DB before login.

```java
@RestController
@RequestMapping("/auth")
@RequiredArgsConstructor
public class AuthController {

    private final UserRepository repo;
    private final PasswordEncoder encoder;

    @PostMapping("/register")
    public String register(@RequestBody User user){

        user.setPassword(
                encoder.encode(user.getPassword())
        );

        user.setRole("ROLE_USER");

        repo.save(user);

        return "User Registered!";
    }
}
```

🔥 Notice:

We encode BEFORE saving.

Always.

---

# Step 9 — Test It

## Register user:

```
POST /auth/register
```

Body:

```json
{
  "email":"test@gmail.com",
  "password":"1234"
}
```

---

## Call Protected API

Create a controller:

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello(){
        return "Secured endpoint!";
    }
}
```

Now access:

```
GET /hello
```

Use Basic Auth:

```
username → email
password → original password
```

If you see the response → 🎉 authentication works.

---

# What You Just Built (Understand This)

You built a REAL authentication pipeline:

```
Basic Auth
   ↓
Security Filter
   ↓
AuthenticationManager
   ↓
DaoAuthenticationProvider
   ↓
UserDetailsService
   ↓
Database
   ↓
BCrypt Check
```

This is EXACTLY how production authentication starts.

---

# NEXT STEP (VERY IMPORTANT)

After this works:

👉 Move to JWT.

Because Basic Auth sends credentials every request.

JWT is what real APIs use.

---

# Biggest Advice I Can Give You

Do NOT jump topic to topic.

Build while learning security.

Security is NOT theoretical knowledge.

It is muscle memory.

---
