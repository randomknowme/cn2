https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3

# unit 2
## **Collision Resolution Techniques in Hashing**
When two keys map to the same index in a **hash table**, a **collision** occurs. To resolve these collisions, we use **collision resolution techniques**.

### **Two main approaches**:
1. **Separate Chaining (Open Hashing)** – Uses linked lists (or other structures) to store multiple elements at the same index.
2. **Open Addressing (Closed Hashing)** – Stores all elements **inside** the hash table itself and finds an alternate slot when a collision occurs.

---

## **1. Separate Chaining (Open Hashing)**  
Instead of storing only **one element** per index, each index **stores a linked list** (or another data structure like a BST) to hold multiple elements.

### **How it Works (Example)**:
Assume a hash function:  
\[
h(k) = k \mod 5
\]
For keys **{10, 15, 20, 25, 30, 35}** in a **hash table of size 5**:

| **Index** | **Linked List (Chained Elements)** |
|----------|--------------------------------|
| 0        | 10 → 15 → 20 → 25 → 30 → 35  |
| 1        | -                              |
| 2        | -                              |
| 3        | -                              |
| 4        | -                              |

Each bucket stores a **linked list** of colliding elements.

### **Advantages**:
✔ **Efficient insertions and deletions** (no need for shifting elements).  
✔ **No limit on elements stored** (only dependent on available memory).  
✔ **Works well even when load factor > 1** (can store multiple elements in the same index).  

### **Disadvantages**:
✘ **Extra memory required** (due to linked list pointers).  
✘ **Poor cache performance** (linked lists may be scattered in memory).  
✘ **Increased search time** (searching inside a long linked list can take O(n) time in the worst case).  
✘ **Wasted space if chains are small or empty**.  

---

## **2. Open Addressing (Closed Hashing)**  
Unlike **separate chaining**, open addressing **stores all elements inside the table itself**. If a collision occurs, it searches for the next available slot using a **probing technique**.

### **Types of Open Addressing**:

### **2.1 Linear Probing**  
When a collision happens, **search for the next empty slot sequentially** (index + 1, index + 2, …).

### **How it Works (Example)**:
Assume a **hash function**:
\[
h(k) = k \mod 7
\]
For keys **{50, 700, 76, 85, 92}** in a **hash table of size 7**:

| **Key** | **h(k) = k mod 7** | **Placement (Linear Probing)** |
|--------|----------------|---------------------------|
| 50     | 50 mod 7 = 1  | Placed at index 1         |
| 700    | 700 mod 7 = 0 | Placed at index 0         |
| 76     | 76 mod 7 = 6  | Placed at index 6         |
| 85     | 85 mod 7 = 1  | Collision at 1 → Move to 2 |
| 92     | 92 mod 7 = 1  | Collision at 1, 2 → Move to 3 |

### **Advantages**:
✔ **Easy to implement** (simple linear search for empty slot).  
✔ **Cache-friendly** (all elements stored in contiguous memory).  

### **Disadvantages**:
✘ **Primary clustering** (groups of occupied slots form, making searches slow).  
✘ **Performance degrades as table fills up** (many collisions lead to long probing sequences).  
✘ **Table size must be large enough to avoid clustering**.  

---

### **2.2 Quadratic Probing**  
Instead of searching **sequentially**, **quadratic probing** checks positions using a quadratic function:
\[
h(k, i) = (h(k) + i^2) \mod m
\]
(where \(i\) is the probe number).

### **How it Works (Example)**:
For keys **{27, 18, 29, 28}** in a **table of size 10**,  
using **h(k) = k mod 10**:

| **Key** | **h(k) = k mod 10** | **Placement (Quadratic Probing)** |
|--------|----------------|---------------------------|
| 27     | 27 mod 10 = 7  | Placed at index 7         |
| 18     | 18 mod 10 = 8  | Placed at index 8         |
| 29     | 29 mod 10 = 9  | Placed at index 9         |
| 28     | 28 mod 10 = 8  | Collision at 8 → Try \(8 + 1^2 = 9\) (collision) → Try \(8 + 2^2 = 12\) → Index 2 |

### **Advantages**:
✔ **Reduces primary clustering** (better distribution of elements).  
✔ **Works better than linear probing when the table is moderately full**.  

### **Disadvantages**:
✘ **Secondary clustering** (keys with the same initial hash probe the same alternative locations).  
✘ **If table size is not a prime number, it may not find an empty slot**.  

---

### **2.3 Double Hashing**  
Instead of using a **fixed interval**, a **second hash function** determines the step size:
\[
h(k, i) = (h_1(k) + i \times h_2(k)) \mod m
\]

### **How it Works (Example)**:
For a **table of size 11**,  
using:
- \(h_1(k) = k \mod 11\)  
- \(h_2(k) = 7 - (k \mod 7)\)

| **Key** | **h₁(k)** | **h₂(k)** | **Placement** |
|--------|----------|----------|--------------|
| 47     | 47 mod 11 = 3  | 7 - (47 mod 7) = 4  | Index 3 |
| 39     | 39 mod 11 = 6  | 7 - (39 mod 7) = 3  | Index 6 |
| 24     | 24 mod 11 = 2  | 7 - (24 mod 7) = 2  | Index 2 |
| 55     | 55 mod 11 = 0  | 7 - (55 mod 7) = 4  | Index 0 |
| 67     | 67 mod 11 = 1  | 7 - (67 mod 7) = 3  | Collision → Try \( (1 + 1×3) \mod 11 = 4 \) |

### **Advantages**:
✔ **Best collision resolution for open addressing**.  
✔ **Avoids both primary and secondary clustering**.  

### **Disadvantages**:
✘ **Extra computation for second hash function**.  
✘ **Choosing a good second hash function is critical**.  

---

## **Comparison Table**

| **Technique** | **Memory Usage** | **Search Time** | **Clustering** | **Extra Computation** |
|--------------|----------------|----------------|----------------|----------------|
| **Separate Chaining** | High (Linked Lists) | O(n) in worst case | No clustering | No extra computation |
| **Linear Probing** | Low | O(1) when empty, O(n) when full | **Primary Clustering** | No extra hash function |
| **Quadratic Probing** | Low | O(1) when empty, O(n) when full | **Secondary Clustering** | No extra hash function |
| **Double Hashing** | Low | O(1) when empty, O(n) when full | No clustering | **Requires extra computation** |

---

## **Conclusion**
- **Separate chaining is best** if **memory is available** and **high insertions are expected**.
- **Double hashing is the best open addressing method** as it avoids clustering.
- **Quadratic probing is better than linear probing** but still has some clustering.
- **Linear probing is the simplest but suffers from primary clustering**.

🚀 **For best performance**, use **double hashing** for open addressing, and **separate chaining** if memory is available.


https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3
