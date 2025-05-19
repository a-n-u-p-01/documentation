##### Many To One
A Many-to-One relationship is used when multiple instances of one entity are associated with a single instance of another entity. This is the inverse of a One-to-Many relationship.
**When to Use a Many-to-One Relationship**

1. **Order Management Systems**: In an e-commerce or order management system, multiple orders can be placed by a single customer. Here, each order is associated with one customer, but one customer can have many orders.
2. **Data Aggregation**: When you need to aggregate data or analyze it in the context of a specific entity. For example, finding all orders for a specific customer.
3. **Data Consistency**: When you want to ensure that multiple records (orders) are consistently associated with a single record (customer) without duplicating customer information in every order.
//customer table
```java
package com.example.demo.entity;  
  
import javax.persistence.*;  
  
@Entity  
public class Customer {  
  
@Id  
@GeneratedValue(strategy = GenerationType.IDENTITY)  
private Long id;  
  
private String name;  
  
// Getters and setters  
}
```

//order table
```java
package com.example.demo.entity;  
  
import javax.persistence.*;  
  
@Entity  
public class Order {  
  
@Id  
@GeneratedValue(strategy = GenerationType.IDENTITY)  //this only works on supportec databases support auto increament column
private Long id;  
  
private String productName;  
  
@ManyToOne  
@JoinColumn(name = "customer_id")  
private Customer customer;  
  
// Getters and setters  
}
```
- `@ManyToOne`: Indicates that many `Order` entities can be associated with one `Customer`.
- - `@JoinColumn(name = "customer_id")`: Specifies that the `customer_id` column in the `Order` table will store the foreign key reference to the `Customer` entity