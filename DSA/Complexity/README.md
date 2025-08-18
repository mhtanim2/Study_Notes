# Complexity in Data Structures and Algorithms (DSA)

Understanding complexity is essential for analyzing the efficiency of algorithms and data structures. Complexity is generally measured in terms of time and space.

---

## 1. Time Complexity

Time complexity describes how the runtime of an algorithm increases with the size of the input (usually denoted as `n`). It is commonly expressed using Big O notation.

### Common Time Complexities

- **O(1) – Constant Time:**  
  The algorithm takes the same amount of time regardless of input size.  
  **Example:** Accessing an element in an array by index.
  ```python
  arr = [1, 2, 3, 4]
  print(arr[2])  # O(1)
  ```

- **O(n) – Linear Time:**  
  The runtime increases proportionally with input size.  
  **Example:** Looping through an array.
  ```python
  for item in arr:
      print(item)  # O(n)
  ```

- **O(n^2) – Quadratic Time:**  
  The runtime increases with the square of the input size.
  > X<sup>n</sup>=m : 2<sup>3</sup>=8   
  **Example:** Nested loops.
  ```python
  for i in arr:
      for j in arr:
          print(i, j)  # O(n^2)
  ```

- **O(log n) – Logarithmic Time:**  
  The runtime `increases logarithmically` as input size increases.
  That means, `Computation decrease exponentially`

  > X<sup>n</sup>=m will explain as Log<sub>X</sub>m=n
  > 2<sup>3</sup>=8 will explain as Log<sub>2</sub>8=3

  **Example:** Binary search.
  ```python
  def binary_search(arr, target):
      left, right = 0, len(arr) - 1
      while left <= right:
          mid = (left + right) // 2
          if arr[mid] == target:
              return mid
          elif arr[mid] < target:
              left = mid + 1
          else:
              right = mid - 1
      return -1  # O(log n)
  ```

---

## 2. Space Complexity

Space complexity measures the amount of memory an algorithm uses as the input size grows.

- **O(1) – Constant Space:**  
  Uses a fixed amount of memory regardless of input size.
- **O(n) – Linear Space:**  
  Memory usage grows linearly with input size.

**Example:**
```python
def sum_array(arr):
    total = 0  # O(1) space
    for num in arr:
        total += num
    return total
```

---

## 3. Why Complexity Matters

- Helps choose the most efficient algorithm for a problem.
- Predicts how algorithms scale with larger inputs.
- Optimizes performance and resource usage.

---

## 4. Summary Table

| Complexity   | Name         | Example Algorithm      |
|--------------|--------------|-----------------------|
| O(1)         | Constant     | Array access          |
| O(log n)     | Logarithmic  | Binary search         |
| O(n)         | Linear       | Linear search         |
| O(n log n)   | Linearithmic | Merge sort, Quick sort|
| O(n^2)       | Quadratic    | Bubble sort           |
| O(2^n)       | Exponential  | Recursive Fibonacci   |

---

Understanding complexity helps you write better, faster, and