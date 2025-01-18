
# Python Quick Reference Guide

## Data Structures

### Lists
- **Creation**
  ```python
  lst = []  # Empty list
  lst = [1, 2, 3]  # With elements
  lst = [0] * 5  # [0, 0, 0, 0, 0]
  ```
- **Basic Operations**
  ```python
  lst[0]  # Access first element
  lst[-1]  # Access last element
  lst.append(x)  # Add to end
  lst.pop()  # Remove & return last element
  lst.pop(0)  # Remove & return first element
  lst.insert(index, x)  # Insert at index
  del lst[1]  # Delete at index
  len(lst)  # Length of list
  ```
- **Slicing**
  ```python
  lst[1:4]  # Elements from index 1 to 3
  lst[:3]  # Elements from start to index 2
  lst[3:]  # Elements from index 3 to end
  lst[::-1]  # Reverse list
  ```

### Dictionary (HashMap)
- **Creation & Basic Operations**
  ```python
  d = {}  # Empty dictionary
  d = {1: "one", 2: "two"}  # With key-value pairs
  d[key] = value  # Add/Update
  d.get(key, default)  # Get value with default
  del d[key]  # Delete key-value pair
  ```
- **Useful Methods**
  ```python
  d.keys()  # Get all keys
  d.values()  # Get all values
  d.items()  # Get all key-value pairs
  ```
- **Frequency Counter Pattern**
  ```python
  freq = {}
  for num in nums:
      freq[num] = freq.get(num, 0) + 1
  ```
- **Counter (from collections)**
  ```python
  from collections import Counter
  nums = [1, 2, 2, 3, 3, 3]
  count = Counter(nums)  # Counter({3: 3, 2: 2, 1: 1})
  ```

## Common Algorithms & Time Complexities

### Sorting
- **Tim Sort (Python's built-in sort)**
  ```python
  lst.sort()  # In-place, O(n log n)
  sorted(lst)  # Returns new list, O(n log n)
  lst.sort(reverse=True)  # Sort in descending order
  lst.sort(key=len)  # Sort by length
  ```

### Heap Operations (heapq)
- **Using Min Heap**
  ```python
  import heapq
  heapq.heappush(heap, item)  # O(log n)
  heapq.heappop(heap)  # O(log n)
  heapq.heapify(lst)  # O(n)
  heap[0]  # Get min element O(1)
  ```
- **For Max Heap**
  ```python
  heapq.heappush(heap, -item)  # Push negative values
  -heapq.heappop(heap)  # Pop and revert
  ```

### Queue Operations (deque)
```python
from collections import deque
q = deque()
q.append(x)  # Add to right, O(1)
q.popleft()  # Remove from left, O(1)
q.appendleft(x)  # Add to left, O(1)
q.pop()  # Remove from right, O(1)
```

## Useful Tricks

### String Operations
```python
s = "hello world"
s = s.strip()  # Remove whitespace
s = s.lower()  # Convert to lowercase
''.join(lst)  # Join list elements into string
s[::-1]  # Reverse string
ord('a')  # Get ASCII value of character
```

### Bit Operations
```python
n & 1  # Check if odd (1) or even (0)
n >> 1  # Divide by 2
n << 1  # Multiply by 2
```

### List Comprehension
```python
[x for x in range(5)]  # [0, 1, 2, 3, 4]
[x for x in lst if x > 0]  # Filter positive numbers
[x if x > 0 else 0 for x in lst]  # With condition
```

## Common Patterns

### Two Pointers
```python
left, right = 0, len(nums) - 1
while left < right:
    # Process elements
    left += 1
    right -= 1
```

### Sliding Window
```python
window = {}
left = curr_sum = 0
for right in range(len(nums)):
    # Add right element to window
    # Process window
    while condition_not_met:
        left += 1
```

### Binary Search
```python
left, right = 0, len(nums) - 1
while left <= right:
    mid = left + (right - left) // 2
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```

## Tips & Best Practices
- Use `Counter` for frequency counting.
- Prefer `deque` over `list` for queue operations.
- Binary search: `left + (right - left) // 2` prevents overflow.
- Sorting complexity: O(n log n).
- Hash map/set operations: O(1).
- Heap push/pop: O(log n).
- Sliding window and two pointers: O(n).
