## 1. Why a New Date–Time API Was Needed

Before Java 8, Java used:

- `java.util.Date`
    
- `java.util.Calendar`
    

### Problems with Old API

- Mutable (thread-unsafe)
    
- Confusing API design
    
- Months are 0-based
    
- Poor timezone handling
    
- Difficult date arithmetic
    
- Formatting mixed with logic
    

Example:

```java
Date d = new Date(2024, 8, 10); // Wrong, confusing
```

Java 8 introduced `java.time` inspired by **Joda-Time** to fix these issues.

---

## 2. Core Design Principles of `java.time`

The new API is:

- Immutable
    
- Thread-safe
    
- Clear separation of concepts
    
- Fluent and readable
    
- ISO-8601 standard based
    

Key idea:

```
Date ≠ Time ≠ TimeZone
```

Each concept has its **own class**.

---

## 3. Key Packages

- `java.time` → Core classes
    
- `java.time.format` → Formatting/parsing
    
- `java.time.temporal` → Date-time fields & units
    
- `java.time.zone` → Time zone rules
    

---

## 4. Fundamental Date–Time Concepts

|Concept|Meaning|
|---|---|
|Date|Year, month, day|
|Time|Hour, minute, second|
|Date-Time|Date + Time|
|Instant|Timestamp (machine time)|
|Zone|Region-based time|
|Offset|Fixed UTC difference|

---

## 5. Core Classes (WHAT EACH REPRESENTS)

### 5.1 `LocalDate`

Represents **date only**, no time, no timezone.

```java
LocalDate date = LocalDate.now();
LocalDate d = LocalDate.of(2024, 10, 5);
```

Use when:

- Birthdays
    
- Holidays
    
- Business dates
    

---

### 5.2 `LocalTime`

Represents **time only**, no date, no timezone.

```java
LocalTime time = LocalTime.now();
LocalTime t = LocalTime.of(14, 30);
```

Use when:

- Store opening times
    
- Schedules
    

---

### 5.3 `LocalDateTime`

Represents **date + time**, no timezone.

```java
LocalDateTime dt = LocalDateTime.now();
```

Use when:

- Database timestamps (without zone)
    
- Local system time
    

---

### 5.4 `Instant`

Represents **machine timestamp** (UTC).

- Epoch-based (1970-01-01T00:00Z)
    
- Nanosecond precision
    

```java
Instant now = Instant.now();
```

Use when:

- Logging
    
- Auditing
    
- Event ordering
    

---

### 5.5 `ZonedDateTime`

Represents **date + time + timezone**.

```java
ZonedDateTime zdt =
    ZonedDateTime.now(ZoneId.of("Asia/Kolkata"));
```

Handles:

- Daylight Saving Time
    
- Region-specific rules
    

---

### 5.6 `OffsetDateTime`

Represents **date + time + UTC offset**.

```java
OffsetDateTime odt =
    OffsetDateTime.now(ZoneOffset.of("+05:30"));
```

Offset is fixed, no DST rules.

---

## 6. Time Zones and ZoneId

### ZoneId

```java
ZoneId zone = ZoneId.of("Asia/Kolkata");
```

List zones:

```java
ZoneId.getAvailableZoneIds();
```

Region-based zones are preferred over offsets.

---

## 7. Immutability (WHY IT MATTERS)

All `java.time` classes are immutable.

```java
LocalDate date = LocalDate.now();
date.plusDays(1); // returns new object
```

Benefits:

- Thread-safe
    
- No side effects
    
- Predictable behavior
    

---

## 8. Date-Time Arithmetic

```java
LocalDate d = LocalDate.now();

d.plusDays(5);
d.minusMonths(1);
d.plusYears(2);
```

Time arithmetic:

```java
LocalTime t = LocalTime.now();
t.plusHours(2);
```

---

## 9. Period vs Duration (IMPORTANT)

### Period

- Date-based
    
- Uses years, months, days
    

```java
Period p = Period.of(1, 2, 10);
```

### Duration

- Time-based
    
- Uses seconds, nanoseconds
    

```java
Duration d = Duration.ofMinutes(30);
```

Rule:

- Use `Period` for dates
    
- Use `Duration` for time
    

---

## 10. Comparing Dates and Times

```java
date.isBefore(other);
date.isAfter(other);
date.isEqual(other);
```

---

## 11. Formatting and Parsing

### DateTimeFormatter

```java
DateTimeFormatter fmt =
    DateTimeFormatter.ofPattern("dd-MM-yyyy");

String s = date.format(fmt);
LocalDate d = LocalDate.parse(s, fmt);
```

Thread-safe (unlike `SimpleDateFormat`).

---

## 12. Parsing ISO Dates

```java
LocalDate.parse("2024-10-05");
```

ISO-8601 by default.

---

## 13. Conversion Between Types

```java
LocalDateTime ldt = LocalDateTime.now();
ZonedDateTime zdt = ldt.atZone(ZoneId.systemDefault());

Instant instant = zdt.toInstant();
```

---

## 14. Interoperability with Old API

```java
Date date = Date.from(Instant.now());
Instant instant = date.toInstant();
```

---

## 15. Date-Time Adjusters

Used for common date rules.

```java
LocalDate firstDay =
    date.with(TemporalAdjusters.firstDayOfMonth());
```

---

## 16. Common Use Cases in Backend

- Database timestamps
    
- Token expiry
    
- Audit logs
    
- Scheduling
    
- Timezone conversion
    
- SLA calculations
    

---

## 17. Common Mistakes

- Using `LocalDateTime` for timezone data
    
- Using `Instant` for business dates
    
- Hardcoding offsets
    
- Mixing old and new APIs
    

---

## 18. Interview-Critical Points

- Why `java.time` is immutable
    
- Difference between `Instant` and `LocalDateTime`
    
- `ZonedDateTime` vs `OffsetDateTime`
    
- `Period` vs `Duration`
    
- Why old `Date` is bad
    

---

## 19. Final Mental Model

```
LocalDate       → Human date
LocalTime       → Human time
LocalDateTime   → Human date-time
Instant         → Machine time
ZonedDateTime   → Real-world time
```

---

## 20. Summary

- `java.time` fixes all old date-time issues
    
- Clean separation of concepts
    
- Immutable and thread-safe
    
- Essential for modern Java backend development
    

---
