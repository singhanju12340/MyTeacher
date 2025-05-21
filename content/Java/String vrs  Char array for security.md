

**`String` is immutable**:

- Once created, it cannot be changed (e.g., replaced password with `****` after use).
- - Stored in the **String Pool** (if interned) or heap. Even if not interned, lives in heap memory until GC runs.
- 
**`char[]` is mutable**:

- You can overwrite it after use (e.g., fill with zeros):
- Contiguous memory block that can be explicitly cleared. No risk of being pooled/cached.
```Java
char[] password = getUserInput();
// Use password...
Arrays.fill(password, '\0'); // Securely wipe from memory
```
```