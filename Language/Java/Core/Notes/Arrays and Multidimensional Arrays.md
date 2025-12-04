An **array** is a **container object** that holds a fixed number of values of a single type. It is a **data structure** that stores elements in **contiguous memory locations**.

### **Key Features:**

1. Fixed size (once created, length cannot be changed).
    
2. Stores elements of the **same type**.
    
3. Supports **random access** using index (0-based).
    
4. Can be **primitive type** or **object type**.
    

---

## **1.1 Syntax**

```java
// Declaration
int[] numbers;
String[] names;

// Initialization
numbers = new int[5];   // Array of 5 integers
names = new String[]{"Alice", "Bob", "Charlie"}; // Array with values

// Declaration + Initialization
int[] nums = {1, 2, 3, 4, 5};
```

---

## **1.2 Accessing Elements**

```java
int[] arr = {10, 20, 30, 40, 50};

// Access by index
System.out.println(arr[0]); // 10
System.out.println(arr[4]); // 50

// Modify element
arr[2] = 100; // arr becomes {10, 20, 100, 40, 50}

// Loop through array
for(int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// Enhanced for loop
for(int val : arr) {
    System.out.println(val);
}
```

---

## **1.3 Important Properties**

- `length`: a built-in property to get size
    

```java
System.out.println(arr.length); // 5
```

- Default values:
    
    - `int`, `byte`, `short`, `long` → `0`
        
    - `float`, `double` → `0.0`
        
    - `char` → `\u0000`
        
    - `boolean` → `false`
        
    - Object types → `null`
        

---

## **1.4 Common Array Operations**

1. **Copying Array**
    

```java
int[] arr2 = Arrays.copyOf(arr, arr.length);
```

2. **Sorting**
    

```java
Arrays.sort(arr);
```

3. **Searching**
    

```java
int index = Arrays.binarySearch(arr, 20); // Array must be sorted
```

4. **Comparing Arrays**
    

```java
boolean equal = Arrays.equals(arr1, arr2);
```

---

# **2. Multidimensional Arrays (2D, 3D, etc.)**

A **multidimensional array** is an array of arrays. The most commonly used is **2D array**, which can be visualized as a **matrix**.

---

## **2.1 Declaration and Initialization**

```java
// 2D array declaration
int[][] matrix;

// 2D array initialization
matrix = new int[3][4]; // 3 rows, 4 columns

// Initialization with values
int[][] mat = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

---

## **2.2 Accessing Elements**

```java
System.out.println(mat[0][1]); // 2 (row 0, column 1)
mat[2][2] = 99; // Change value at row 2, column 2
```

---

## **2.3 Looping Through 2D Arrays**

**Using nested loops:**

```java
for(int i = 0; i < mat.length; i++) {        // rows
    for(int j = 0; j < mat[i].length; j++) { // columns
        System.out.print(mat[i][j] + " ");
    }
    System.out.println();
}
```

**Using enhanced for loop:**

```java
for(int[] row : mat) {
    for(int val : row) {
        System.out.print(val + " ");
    }
    System.out.println();
}
```

---

## **2.4 Jagged Arrays**

- A jagged array is an array of arrays **with different column lengths**.
    

```java
int[][] jagged = new int[3][];
jagged[0] = new int[2]; // row 0 has 2 columns
jagged[1] = new int[4]; // row 1 has 4 columns
jagged[2] = new int[3]; // row 2 has 3 columns
```

---

## **2.5 Multidimensional Arrays in Memory**

- 2D arrays are **arrays of arrays**.
    
- `matrix[0]` is actually a reference to a 1D array representing row 0.
    
- Elements are stored in **heap memory**, but each row can have different length (jagged array).
    

---

# **3. Common Operations on Multidimensional Arrays**

1. **Transpose of a Matrix**
    

```java
int[][] transpose = new int[cols][rows];
for(int i=0; i<rows; i++){
    for(int j=0; j<cols; j++){
        transpose[j][i] = mat[i][j];
    }
}
```

2. **Matrix Addition**
    

```java
int[][] sum = new int[rows][cols];
for(int i=0; i<rows; i++){
    for(int j=0; j<cols; j++){
        sum[i][j] = mat1[i][j] + mat2[i][j];
    }
}
```

3. **Matrix Multiplication**
    

```java
int[][] product = new int[rows1][cols2];
for(int i=0; i<rows1; i++){
    for(int j=0; j<cols2; j++){
        for(int k=0; k<cols1; k++){
            product[i][j] += mat1[i][k] * mat2[k][j];
        }
    }
}
```

---

# **4. Advantages of Arrays**

- Fast **random access** using index.
    
- Simple and memory-efficient for **fixed-size data**.
    
- Useful in **mathematical computations** and storing **related data**.
    

---

# **5. Limitations of Arrays**

- Fixed size → cannot dynamically grow or shrink.
    
- Insertion and deletion in the middle is costly → requires shifting elements.
    
- Primitive arrays don’t support advanced operations without loops.
    

---
### **1. Basic Questions**

##### **Q1: How do you declare and initialize an array in Java?**

**Answer:**

```java
// Declaration
int[] arr1;
String[] names;

// Initialization
arr1 = new int[5];                // Array of 5 integers (default 0)
String[] fruits = {"Apple", "Banana", "Mango"}; // Array with values

// Declaration + Initialization
int[] numbers = {1, 2, 3, 4, 5};
```

---

#### **Q2: Difference between `arr.length` and `arr.length()`**

**Answer:**

- `arr.length` → **Property of array** (no parentheses). Gives the size of the array.
    
- `str.length()` → **Method of String** (parentheses needed). Gives the length of the string.
    

```java
int[] arr = {1,2,3};
System.out.println(arr.length); // 3

String s = "Hello";
System.out.println(s.length()); // 5
```

---

### **2. Intermediate Questions**

##### **Q3: Reverse an array without using additional arrays**

```java
int[] arr = {1, 2, 3, 4, 5};
int n = arr.length;

for (int i = 0; i < n / 2; i++) {
    int temp = arr[i];
    arr[i] = arr[n - 1 - i];
    arr[n - 1 - i] = temp;
}

// arr becomes {5, 4, 3, 2, 1}
```

---

#### **Q4: Find largest and smallest element in an array**

```java
int[] arr = {5, 2, 9, 1, 7};
int max = arr[0], min = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) max = arr[i];
    if (arr[i] < min) min = arr[i];
}

System.out.println("Max: " + max + ", Min: " + min);
```

---

## **3. Advanced Questions**

##### **Q5: Find the second largest element**

```java
int[] arr = {5, 2, 9, 1, 7};
int first = Integer.MIN_VALUE, second = Integer.MIN_VALUE;

for(int num : arr){
    if(num > first){
        second = first;
        first = num;
    } else if(num > second && num != first){
        second = num;
    }
}

System.out.println("Second Largest: " + second); // 7
```

---

##### **Q6: Rotate an array by `k` positions (right rotation)**

```java
int[] arr = {1, 2, 3, 4, 5};
int k = 2;
int n = arr.length;

// Create temp array
int[] temp = new int[n];
for (int i = 0; i < n; i++) {
    temp[(i + k) % n] = arr[i];
}

arr = temp; // arr becomes {4, 5, 1, 2, 3}
```

---

##### **Q7: Transpose of a 2D array**

```java
int[][] mat = {
    {1, 2, 3},
    {4, 5, 6}
};

int rows = mat.length;
int cols = mat[0].length;
int[][] transpose = new int[cols][rows];

for(int i=0; i<rows; i++){
    for(int j=0; j<cols; j++){
        transpose[j][i] = mat[i][j];
    }
}

// transpose = {{1,4},{2,5},{3,6}}
```

---

##### **Q8: Difference between rectangular and jagged array**

| Aspect     | Rectangular Array                    | Jagged Array                                                            |
| ---------- | ------------------------------------ | ----------------------------------------------------------------------- |
| Definition | All rows have same number of columns | Rows can have different number of columns                               |
| Memory     | Continuous 2D structure              | Array of arrays                                                         |
| Example    | `int[][] arr = new int[3][4];`       | `int[][] arr = new int[3][]; arr[0] = new int[2]; arr[1] = new int[5];` |

---

#### **Q9: Sum of diagonal elements in 2D array**

```java
int[][] mat = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

int sum = 0;
for(int i=0; i<mat.length; i++){
    sum += mat[i][i]; // Primary diagonal
}

System.out.println("Sum of diagonal: " + sum); // 1+5+9=15
```

---

#### **Q10: Memory allocation of 2D arrays in Java**

- 2D arrays in Java are **arrays of arrays**.
    
- `matrix[0]` is a **reference to a 1D array** representing row 0.
    
- Can be **rectangular** (all rows same length) or **jagged** (rows different lengths).
    
- Stored in **heap memory**, not stack.
    

---

# **4. Tricky / Problem-Solving Questions**

### **Q11: Find duplicate elements in an array**

```java
int[] arr = {1, 2, 3, 2, 4, 5, 1};
Set<Integer> set = new HashSet<>();
for(int num : arr){
    if(!set.add(num)){
        System.out.println("Duplicate: " + num);
    }
}
// Output: 2, 1
```

---

### **Q12: Move all zeros to the end**

```java
int[] arr = {0,1,0,3,12};
int n = arr.length, j=0;

for(int i=0;i<n;i++){
    if(arr[i]!=0){
        arr[j++] = arr[i];
    }
}
while(j<n){
    arr[j++] = 0;
}

// arr = {1,3,12,0,0}
```

---

### **Q13: Maximum sum subarray (Kadane’s Algorithm)**

```java
int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
int maxSoFar = arr[0], maxEndingHere = arr[0];

for(int i=1; i<arr.length; i++){
    maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
}

System.out.println("Maximum sum subarray: " + maxSoFar); // 6
```

---