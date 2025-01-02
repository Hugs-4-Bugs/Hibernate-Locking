
# Hibernate Locking

Hibernate locking ensures data consistency and prevents conflicts when multiple transactions access the same data concurrently. It controls how entities are accessed, modified, and persisted to the database by managing concurrent transactions.  
Locking can be classified into two main types:
* Optimistic Locking
* Pessimistic Locking


## 1. Optimistic Locking

### When to Use:
* When the likelihood of conflicts is low.
* Suitable for read-heavy applications.
* Best for scenarios where transactions are short-lived and the application can handle occasional retries.

### Why to Use:
* Prevents dirty reads and ensures data integrity.
* No database-level locking, improving performance by avoiding lock contention.
* Lightweight and scalable for applications with minimal data conflicts.

### How to Use (Implementation):
1. Add a @Version field to the entity.
2. The version is automatically updated during each transaction. If another transaction modifies the same record, an exception is thrown.

Example:

```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private double price;

    @Version  // Optimistic Locking
    private int version;

    // Getters and Setters
}
```

Transaction Example:

```java
@Transactional
public void updateProduct(Long productId, double newPrice) {
    Product product = entityManager.find(Product.class, productId);
    product.setPrice(newPrice);
    entityManager.merge(product);
}
```

If two users try to update the same product, one transaction will fail with an `OptimisticLockException`.

---

## 2. Pessimistic Locking

### When to Use:
* When the likelihood of conflicts is high.
* Suitable for write-heavy applications.
* Use when transactions involve complex data manipulations that require consistency.

### Why to Use:
* Prevents concurrent modifications by locking the record until the transaction completes.
* Ensures strong data consistency and avoids lost updates.
* Useful when data integrity is more important than performance.

### How to Use (Implementation):
1. Use `LockModeType.PESSIMISTIC_WRITE` or `LockModeType.PESSIMISTIC_READ` to acquire locks.
2. Hibernate issues `SELECT ... FOR UPDATE` queries to lock records.

Example:

```java
@Transactional
public Product getProductWithLock(Long productId) {
    return entityManager.find(Product.class, productId, LockModeType.PESSIMISTIC_WRITE);
}
```

Transaction Example:

```java
@Transactional
public void updateProductStock(Long productId) {
    Product product = entityManager.find(Product.class, productId, LockModeType.PESSIMISTIC_WRITE);
    product.setPrice(product.getPrice() + 10);
    entityManager.merge(product);
}
```

The record is locked, preventing other transactions from modifying it until the current one finishes.




## Types of Lock Modes in Hibernate

<img width="394" alt="Screenshot 2024-12-31 at 6 49 15 PM" src="https://github.com/user-attachments/assets/b3156af3-80a9-47df-b592-9c383212a580" />




## Optimistic Locking

### Advantages:
- Higher performance in low-contention scenarios.
- No need for database-level locks.
- Scalable for distributed applications.

### Disadvantages:
- Transactions may fail and need to be retried.
- Not suitable for high-contention systems.

## Pessimistic Locking

### Advantages:
- Ensures data consistency by preventing concurrent updates.
- Suitable for high-contention scenarios.

### Disadvantages:
- Reduces performance due to database-level locks.
- Risk of deadlocks.
- Not scalable for large applications.

## Key Use Cases

### Optimistic Locking:
- E-commerce applications for updating product inventory.
- Social media posts where concurrent edits are rare.

### Pessimistic Locking:
- Banking transactions to prevent balance inconsistencies.
- Ticket booking systems where concurrency is high.

## Answers to Questions to Consider

1. **What is the primary purpose of Hibernate locking?**
   - To ensure data consistency and prevent conflicts in concurrent transactions by managing how entities are accessed and modified.

2. **Explain the difference between optimistic and pessimistic locking.**
   - **Optimistic Locking**: Assumes low conflict; uses versioning to detect conflicts at the end of a transaction.
   - **Pessimistic Locking**: Assumes high conflict; locks data at the start of a transaction to prevent concurrent access.

3. **When should optimistic locking be preferred over pessimistic locking?**
   - Use optimistic locking in read-heavy, low-contention environments where the chance of concurrent updates is minimal.

4. **What annotations are used to implement optimistic locking in Hibernate?**
   - `@Version` is used to implement optimistic locking.

5. **Describe a use case where pessimistic locking is essential.**
   - Pessimistic locking is essential in banking systems to prevent balance inconsistencies during concurrent transactions.

6. **What are the advantages and disadvantages of optimistic locking?**
   - **Advantages**: High performance, no locks.
   - **Disadvantages**: Transaction failures in high-contention scenarios.

7. **How does pessimistic locking ensure data consistency in concurrent transactions?**
   - By locking the record, preventing others from modifying it until the transaction completes.

8. **List and explain the different lock modes available in Hibernate.**
   - See section 4 (Types of Lock Modes in Hibernate).

9. **Provide an example of how to implement pessimistic locking in Hibernate.**
   - See section 3 (Pessimistic Locking Example).

10. **Why might optimistic locking fail, and how can it be handled?**
   - It fails if concurrent transactions modify the same record. Handle by catching `OptimisticLockException` and retrying the transaction.


---


## Click here to get in touch with me: 
<a href="https://github.com/Tech-Hubs" target="_blank"><b>PrabhatDevLab</b></a>, 
<a href="https://hugs-4-bugs.github.io/myResume/" target="_blank"><b>PrabhatKumar.com</b></a>, 
<a href="https://www.linkedin.com/in/prabhat-kumar-6963661a4/" target="_blank"><b>LinkedIn</b></a>, 
<a href="https://stackoverflow.com/users/19520484/prabhat-kumar" target="_blank"><b>Stackoverflow</b></a>, 
<a href="https://github.com/Hugs-4-Bugs" target="_blank"><b>GitHub</b></a>, 
<a href="https://leetcode.com/u/Hugs-2-Bugs/" target="_blank"><b>LeetCode</b></a>, 
<a href="https://www.hackerrank.com/profile/Prabhat_7250" target="_blank"><b>HackerRank</b></a>, 
<a href="https://www.geeksforgeeks.org/user/stealthy_prabhat/" target="_blank"><b>GeeksforGeeks</b></a>, 
<a href="https://hugs-4-bugs.github.io/AlgoByPrabhat/" target="_blank"><b>AlgoByPrabhat</b></a>, 
<a href="http://hugs-4-bugs.github.io/Sharma-AI/" target="_blank"><b>SHARMA AI</b></a>,  <a href="https://linktr.ee/_s_4_sharma" target="_blank"><b>About Me</b></a>, <a href="https://www.instagram.com/_s_4_sharma/" target="_blank"><b>Instagram</b></a>, <a href="https://x.com/kattyPrabhat" target="_blank"><b>Twitter</b></a>



<p>Happy Learning! 📚✨ Keep exploring and growing your knowledge! 🚀😊</p>
