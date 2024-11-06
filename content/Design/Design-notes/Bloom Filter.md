---
Creation Time: Monday, June 24th 2024
Modified Time: Wednesday, October 23rd 2024
---
Ex: Strong Password verification

`A Bloom filter is a space-efficient probabilistic data structure used to test whether an element is a member of a set

**Here's how it works:**

1. **Initialization:**
    
    - **Bit array:** A bit array of a fixed size is created.
    - **Hash functions:** A set of hash functions is chosen. These functions should be independent and have a uniform distribution.
2. **Insertion:**
    
    - When an element is inserted into the Bloom filter:
        - It is hashed using each of the hash functions.
        - The resulting hash values are used as indices into the bit array.
        - The corresponding bits in the bit array are set to 1.
3. **Membership testing:**
    
    - To check if an element is in the set:
        - It is hashed using each of the hash functions.
        - The resulting hash values are used as indices into the bit array.
        - If all corresponding bits are 1, the element is **probably** in the set. However, there's a chance of a false positive (the element is not in the set but all bits are 1).

_**Applications:**

- **Database indexing:** To check if a value exists in a large database.
- **Network filtering:** To filter out known spam or malicious traffic.
- **Data deduplication:** To identify duplicate data in large datasets.
- **Web caching:** To check if a web page is in the cache.