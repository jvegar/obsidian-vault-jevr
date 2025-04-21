Certainly! Logical comparisons in shell scripting are crucial for controlling script flow and decision-making. Here's a comprehensive breakdown:

---

### **1. Basic Comparison Syntax**
Use `test` command or its alias `[ ]` (with spaces):
```bash
[ condition ]
# OR
test condition
```

For advanced features, use `[[ ]]` (Bash and modern shells):
```bash
[[ condition ]]
```

---

### **2. Numeric Comparisons**
Use these operators with **numbers**:
- `-eq` Equal to (`==`)
- `-ne` Not equal to (`!=`)
- `-lt` Less than (`<`)
- `-le` Less than or equal to (`<=`)
- `-gt` Greater than (`>`)
- `-ge` Greater than or equal to (`>=`)

**Example:**
```bash
if [ $num -gt 10 ]; then
  echo "Number is greater than 10"
fi
```

---

### **3. String Comparisons**
Use these operators with **strings**:
- `=` or `==` Equal to
- `!=` Not equal to
- `-z` String is empty (zero length)
- `-n` String is not empty

**Example:**
```bash
if [[ "$name" = "Alice" ]]; then
  echo "Hello, Alice!"
fi
```

---

### **4. File Tests**
Check file/directory properties:
- `-e` File exists
- `-f` Regular file (not directory)
- `-d` Directory exists
- `-r` Readable
- `-w` Writable
- `-x` Executable
- `-s` File size > 0 bytes

**Example:**
```bash
if [ -f "/path/to/file.txt" ]; then
  echo "File exists"
fi
```

---

### **5. Logical Operators**
Combine conditions:
- `&&` AND (in `[[ ]]` or between commands)
- `||` OR (in `[[ ]]` or between commands)
- `-a` AND (inside `[ ]`)
- `-o` OR (inside `[ ]`)
- `!` NOT

**Examples:**
```bash
# Using [[ ]]
if [[ $num -ge 18 && "$status" = "active" ]]; then
  echo "Eligible"
fi

# Using [ ]
if [ $num -ge 18 -a "$status" = "active" ]; then
  echo "Eligible"
fi

# Inverting a condition
if ! [ -d "/invalid_dir" ]; then
  echo "Directory doesn't exist"
fi
```

---

### **6. Arithmetic Comparisons with (( ))**
Use C-style syntax for numbers:
```bash
if (( num > 10 && num < 20 )); then
  echo "Between 10 and 20"
fi
```

---

### **7. Common Pitfalls**
- **Spacing:** `[ $a = 5 ]` (spaces required around `=` and `]`)
- **Quoting variables:** `"$var"` to prevent word splitting
- **Zero vs. Non-zero:** In shell, `0` = success (true), `1` = failure (false)

---

### **Practical Example**
```bash
#!/bin/bash

file="/path/to/data.txt"
max_attempts=3

if [[ -f "$file" && -r "$file" ]] && (( max_attempts < 5 )); then
  echo "File is readable and attempts are within limit"
else
  echo "Conditions not met"
fi
```

---

### **Key Takeaways**
- Use `[[ ]]` for strings/advanced logic (Bash)
- Use `[ ]` for POSIX compliance
- Prefer `(( ))` for arithmetic operations
- Always quote variables to handle spaces/special characters

Would you like to explore any specific aspect further?