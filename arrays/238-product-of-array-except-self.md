# LeetCode 238 — Product of Array Except Self

## Problem Summary

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all elements of `nums` except `nums[i]`.

**Important:** You must solve the problem without using division and with \(O(n)\) time complexity.

---

## Example

### Input
`nums = [1,2,3,4]`

### Output
`[24,12,8,6]`

### Explanation
- Index 0: `2 * 3 * 4 = 24`
- Index 1: `1 * 3 * 4 = 12`
- Index 2: `1 * 2 * 4 = 8`
- Index 3: `1 * 2 * 3 = 6`

---

## Key Idea

For each element, we need the product of all elements to its **left** and all elements to its **right**.

We can calculate these products using two arrays:

1. **Prefix Product**
   - Stores the product of all elements before the current index.
   - Example: `[1,2,3,4]` → `[1,1,2,6]`

2. **Suffix Product**
   - Stores the product of all elements after the current index.
   - Example: `[1,2,3,4]` → `[24,12,4,1]`

The final answer is:

`result[i] = prefixArr[i] * suffixArr[i]`

---

## Approach

### Step 1 — Initialize the Arrays

Create two arrays of length `n`, filled with `1`:
- `prefixArr` stores prefix products.
- `suffixArr` stores suffix products.

Initialize an empty `result` array.

```python
prefixArr = [1] * l
suffixArr = [1] * l
result = []
```

We use `1` because it is the identity element for multiplication. It allows the first prefix and last suffix to have no elements on one side.

### Step 2 — Calculate Prefix Products

Traverse from left to right, starting at index `1`.

```python
for i in range(1, l):
    prefixArr[i] = prefixArr[i-1] * nums[i-1]
```

Each position contains the product of all elements before it.

For `nums = [1,2,3,4]`:

`prefixArr = [1,1,2,6]`

### Step 3 — Calculate Suffix Products

Traverse from right to left, starting at the second-last index.

```python
for i in range(l-2, -1, -1):
    suffixArr[i] = suffixArr[i+1] * nums[i+1]
```

Each position contains the product of all elements after it.

For `nums = [1,2,3,4]`:

`suffixArr = [24,12,4,1]`

### Step 4 — Calculate the Result

Multiply the prefix and suffix products at each index.

```python
for i in range(0, l):
    result.append(prefixArr[i] * suffixArr[i])
```

The result is:

`[24,12,8,6]`

### Step 5 — Return the Result

Return the final array:

```python
return result
```

---

## Why Prefix and Suffix Arrays?

They allow us to calculate the product excluding the current element without repeatedly traversing the entire array.

For each index, we simply multiply:
- The product of elements on the left.
- The product of elements on the right.

This avoids nested loops and achieves linear time complexity.

---

## Time Complexity

If:
- `n` = number of elements in `nums`

Then:
- Calculating prefix products takes `O(n)`.
- Calculating suffix products takes `O(n)`.
- Building the result takes `O(n)`.

### Total:
`O(n)`

---

## Space Complexity

- `prefixArr` takes `O(n)` space.
- `suffixArr` takes `O(n)` space.
- `result` takes `O(n)` space.

### Total:
`O(n)`

*Note: This is the space complexity of your implementation, including the output array. The problem's follow-up asks whether the solution can use O(1) auxiliary space, excluding the output array.*

---

## Clean Python Solution

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        l = len(nums)
        prefixArr = [1] * l
        suffixArr = [1] * l
        result = []

        for i in range(1, l):
            prefixArr[i] = prefixArr[i-1] * nums[i-1]

        for i in range(l-2, -1, -1):
            suffixArr[i] = suffixArr[i+1] * nums[i + 1]

        for i in range(0, l):
            result.append(prefixArr[i] * suffixArr[i])

        return result
```

---

## Key Takeaway

**Prefix product × Suffix product = Product except self**

This problem demonstrates how precomputing results from the left and right can eliminate repeated calculations and improve an algorithm from \(O(n^2)\) to \(O(n)\) time complexity.
