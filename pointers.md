
## Pointers in C – Explained Step-by-Step

---

### 1. Start with the Definition

 “In C, a **pointer** is a variable that stores the **memory address** of another variable.  


---

### 2. Show the Syntax

```c
int a = 10;
int *ptr = &a;
```

###  Explanation:

- `int` → data type of the variable `a`.
- `*ptr` → declares a pointer to an `int`.
- `&a` → gives the memory address of `a`, which is stored in `ptr`.

---

### 3. Talk About the Operators

| Operator | Meaning                                 |
|----------|------------------------------------------|
| `*`      | Pointer declaration or **dereferencing** |
| `&`      | Address-of operator (gets memory address)|

---

### 4. Explain with a Memory Example

```c
int a = 5;
int *p = &a;
```

### Imagine:

- `a` is stored at address `0x1000`
- `p` holds `0x1000` (the address of `a`)
- `*p` gives the value at that address → `5`

### So:

```c
printf("%d", *p);  // outputs 5
```

---

### 5. Show a Real Code Example

```c
#include <stdio.h>

int main() {
    int a = 10;
    int *p = &a;

    printf("Value of a: %d\n", a);
    printf("Address of a: %p\n", &a);
    printf("Value stored in pointer p: %p\n", p);
    printf("Value pointed to by p: %d\n", *p);

    return 0;
}
```

---

 Output Example (format may vary depending on system):

```
Value of a: 10
Address of a: 0x7ffee914c69c
Value stored in pointer p: 0x7ffee914c69c
Value pointed to by p: 10
```

---
#### 6. What Is a String in C?
A string in C is an array of characters ending with a null character ''.
 C does not have a built-in string type like Python or Java.
Example
```c
char str[] = "hello";
This is internally:
['h', 'e', 'l', 'l', 'o', '\0']
```
#### 7. What Is a String Pointer?
A string pointer is a pointer that points to a character array (a string).
```c
char *str = "hello";
```
	"hello" is a string literal stored in read-only memory
	str points to the first character 'h'
 You should not modify string literals. This is undefined behavior.
#### Declaring String Pointers
1. Using Character Array (modifiable)
```c
char str[] = "hello";
```
2. Using Pointer (read-only)
```c
char *str = "hello";
```
#### Example: Accessing Characters via Pointer
```c
char *str = "hello";

printf("%c\n", str[0]);      // Output: h
printf("%c\n", *(str + 1));  // Output: e
```
#### 8.Pointer to Array of Strings
Declaration
```c
char *words[] = { "one", "two", "three" };
```
words is an array of string pointers
Each words[i] is a char* pointing to a string
#### Example
```c
printf("%s\n", words[1]);  // Output: two
```
#### 9.Memory Layout of char *words[]
words[0] ───→ "one"   → ['o', 'n', 'e', '\0']

words[1] ───→ "two"   → ['t', 'w', 'o', '\0']

words[2] ───→ "three" → ['t', 'h', 'r', 'e', 'e', '\0']
#### Difference Between Static Array and Pointer
```c
	char str[] = "abc";	char *str = "abc";
```
Memory	Stack	Read-only data section
Allocated at	Compile time	Compile time
#### Dynamic String Allocation with malloc()
When the string size is not known at compile time, use malloc():
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Enter length: ");
    scanf("%d", &n);

    char *str = (char *)malloc(n * sizeof(char));
    if (str == NULL) {
        printf("Memory allocation failed.\n");
        return 1;
    }

    printf("Enter string: ");
    scanf("%s", str);

    printf("You entered: %s\n", str);
    free(str);
    return 0;
}
```
#### Resize String at Runtime using realloc()
```c
str = realloc(str, new_size * sizeof(char));
```
•	Works only with memory allocated by malloc()
•	 Cannot be used with static arrays like char str[100];
#### Common Mistakes
Mistake	Why it’s wrong
```c
char *str = "hello"; 
str[0] = 'H';	
```
Modifying read-only memory
```c
char str[5]; 
scanf("%s", str);	
```
No boundary check → buffer overflow


### 9.5 Array of Strings (2D Character Array)

##  Declaration

```c
char arr[5][10] = {
    "white", "red", "green", "yellow", "blue"
};
```

- `arr[i]` = i-th string
- `arr[i][j]` = j-th character in i-th string

---

##  Memory Layout (Assuming 10 bytes per row)

| Address | String   |
| ------- | -------- |
| 2000    | "white"  |
| 2010    | "red"    |
| 2020    | "green"  |
| 2030    | "yellow" |
| 2040    | "blue"   |


---

##  Invalid Assignments

```c
arr[0] = "white";   // ❌ Invalid
arr[1] = arr[2];    // ❌ Invalid
```

Use `strcpy()` or `scanf()`:

```c
strcpy(arr[0], "white"); // Valid
scanf("%s", arr[1]);     // Valid
```

---

###  Example: Printing Strings

```c
#include <stdio.h>
#define N 5
#define LEN 10

int main() {
    char arr[N][LEN] = { "white", "red", "green", "yellow", "blue" };
    for (int i = 0; i < N; i++) {
        printf("String = %s\t", arr[i]);
        printf("Address = %u\n", arr[i]);
    }
    return 0;
}
```

---

### 9.6 Array of Pointers to Strings

####  Declaration

```c
char *arrp[] = { "white", "red", "green", "yellow", "blue" };
```

- `arrp[i]` stores address of string
- Strings may not be stored in contiguous memory
- Total memory: 28 bytes (strings) + 10 bytes (pointers) = **38 bytes**

---

##  Example: Print Strings and Addresses

```c
#include <stdio.h>

int main() {
    char *arrp[] = { "white", "red", "green", "yellow", "blue" };
    for (int i = 0; i < 5; i++) {
        printf("String: %s\t", arrp[i]);
        printf("Address of string: %u\t", arrp[i]);
        printf("Pointer stored at: %u\n", &arrp[i]);
    }
    return 0;
}
```

---

### Valid/Invalid Assignments

| Operation                            | Validity                 |
| ------------------------------------ | ------------------------ |
| `arr[0] = "January";`                |  Invalid                 |
| `arrp[0] = "January";`               |  Valid                   |
| `strcpy(arr[1], "Feb");`             |  Valid                   |
| `strcpy(arrp[1], "Feb");`            |  Invalid if not malloc'd |
| `scanf("%s", arrp[2]);`              |  Invalid if not malloc'd |
| `arrp[3] = malloc(10); strcpy(...);` |  Valid                   |




##  Valid/Invalid Assignments With Code Examples

###  `arr[0] = "January"; //  Invalid`

```c
char arr[5][10];
arr[0] = "January"; // Error: incompatible types in assignment
```

###  `arrp[0] = "January"; //  Valid`

```c
char *arrp[5];
arrp[0] = "January"; // Valid: arrp[0] now points to string constant "January"
```

###  `strcpy(arr[1], "Feb"); //  Valid`

```c
char arr[5][10];
strcpy(arr[1], "Feb"); // Valid: copies "Feb" into arr[1]
```

###  `strcpy(arrp[1], "Feb"); //  Invalid if not malloc

```c
char *arrp[5];
strcpy(arrp[1], "Feb"); // Invalid: arrp[1] is uninitialized (wild pointer)
```

###  `scanf("%s", arrp[2]); //  Invalid if not malloc'd`

```c
char *arrp[5];
scanf("%s", arrp[2]); // Invalid: arrp[2] points to unknown memory
```

### `arrp[3] = malloc(10); strcpy(...); // Valid`

```c
#include <stdlib.h>
#include <string.h>
char *arrp[5];
arrp[3] = (char *)malloc(10);
strcpy(arrp[3], "Hello"); // Valid: memory allocated and string copied
```

---

### Dynamic String Input Example

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char *arrp[10], str[20];
    for (int i = 0; i < 10; i++) {
        printf("Enter string %d: ", i + 1);
        gets(str); // Use fgets() in practice
        arrp[i] = (char *)malloc(strlen(str) + 1);
        strcpy(arrp[i], str);
    }

    printf("\nEntered Strings:\n");
    for (int i = 0; i < 10; i++) {
        printf("%s\t", arrp[i]);
        free(arrp[i]); // Free allocated memory
    }
    return 0;
}
```



