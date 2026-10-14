# LeetCode 49 — Group Anagrams

## Problem Summary

Given an array of strings `strs`, group the anagrams together and return them in any order.

Anagrams are strings that contain the same characters with the same frequency, just arranged differently.

---

## Example

### Input
`["eat","tea","tan","ate","nat","bat"]`

### Output
`[["eat","tea","ate"],["tan","nat"],["bat"]]`

---

## Key Idea

I found two ways to check Anagram.

1. **Sort the string**
   - `"eat"` -> `"aet"`
   - `"tea"` -> `"aet"`

2. **Use character frequency**
   - count letters and use the count as a key

I preferred the **Sort the string** method.

---

## Approach

### Step 1
Create an empty Dictionary where -
- **key** = sorted version of the string
- **value** = list of strings that match that key

### Step 2
For each string:
- sort its characters
- use the sorted result as the map key
- If any other sorted result matches the result then append the original string to that group

### Step 3
Return all grouped values from the Dictionary

## Why Hash Map?

A hash map is used because:
- the key is not an array index
- the key can be a string
- lookup and insertion are fast

---

## Time Complexity

If:
- `n` = number of strings
- `k` = maximum length of a string

Then:
- sorting each string takes `O(k log k)`
- doing this for all strings gives `O(n * k log k)`

### Total:
`O(n * k log k)`

---

## Space Complexity

- Hash map stores all strings
- Sorted keys also take space

### Total:
`O(n * k)`

---

## Clean Python Solution

```python
class Solution:
    def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
        tempDict = {}
        for i,s in enumerate(strs):
            newArr = list(s)
            newArr.sort()
            newStr = "".join(newArr)
            if newStr in tempDict:
                tempDict[newStr].append(s)
            else:
                tempDict[newStr] = [s]
        
        return list(tempDict.values())
