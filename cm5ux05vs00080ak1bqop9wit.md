---
title: "The Sliding Window Technique"
datePublished: Mon Jan 13 2025 10:42:22 GMT+0000 (Coordinated Universal Time)
cuid: cm5ux05vs00080ak1bqop9wit
slug: the-sliding-window-technique
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736764903557/af012b4b-2880-43ba-9372-65b4c033afd3.jpeg
tags: csharp, algorithms, python

---

The Sliding Window technique is a powerful algorithmic approach to solve problems involving arrays, strings, or sequences. It reduces the need for nested loops, improving efficiency and making your code more elegant. This article will introduce the concept, explore common use cases, and demonstrate its implementation in Python and C#.

## What is the Sliding Window Technique?

The Sliding Window technique involves maintaining a subset (or "window") of elements in an array or string and systematically moving it through the dataset. The window can be of fixed size (fixed window) or dynamic size (variable window), depending on the problem requirements.

### Key Features:

* **Start and End Pointers:** Typically represented by two indices that define the bounds of the current window.
    
* **Dynamic Adjustments:** The size or position of the window changes dynamically based on the problem constraints.
    
* **Efficiency:** Eliminates unnecessary recomputation by reusing information from the previous window.
    

## Common Use Cases

1. **Finding Maximum/Minimum in a Subarray of Size K**
    
2. **Checking for Substrings Matching Certain Criteria**
    
3. **Finding the Longest or Shortest Substring with Specific Properties**
    
4. **Calculating Subarray Sums or Averages**
    

## Sliding Window Algorithm Steps

1. **Initialize Variables:** Start and end pointers, along with variables to track the desired result (e.g., sum, length, or max).
    
2. **Expand the Window:** Move the end pointer to include more elements.
    
3. **Shrink the Window (if necessary):** Adjust the start pointer to maintain problem constraints.
    
4. **Update Result:** Update the result variable as needed based on the current window.
    

## Example 1: Maximum Sum of Subarray of Size K

### Problem Statement

Given an array and an integer `k`, find the maximum sum of any contiguous subarray of size `k`.

### Python Implementation

```python
def max_subarray_sum(arr, k):
    max_sum = float('-inf')
    current_sum = 0
    start = 0

    for end in range(len(arr)):
        current_sum += arr[end]  # Add the next element to the current window

        if end >= k - 1:  # Check if window size equals k
            max_sum = max(max_sum, current_sum)
            current_sum -= arr[start]  # Remove the first element of the window
            start += 1  # Slide the window forward

    return max_sum

# Example usage
arr = [2, 1, 5, 1, 3, 2]
k = 3
print(max_subarray_sum(arr, k))  # Output: 9
```

### C# Implementation

```csharp
using System;

class SlidingWindow
{
    public static int MaxSubarraySum(int[] arr, int k)
    {
        int maxSum = int.MinValue;
        int currentSum = 0;
        int start = 0;

        for (int end = 0; end < arr.Length; end++)
        {
            currentSum += arr[end];  // Add the next element to the current window

            if (end >= k - 1)  // Check if window size equals k
            {
                maxSum = Math.Max(maxSum, currentSum);
                currentSum -= arr[start];  // Remove the first element of the window
                start++;  // Slide the window forward
            }
        }

        return maxSum;
    }

    static void Main()
    {
        int[] arr = { 2, 1, 5, 1, 3, 2 };
        int k = 3;
        Console.WriteLine(MaxSubarraySum(arr, k));  // Output: 9
    }
}
```

## Example 2: Longest Substring with K Distinct Characters

### Problem Statement

Given a string and an integer `k`, find the length of the longest substring containing exactly `k` distinct characters.

### Python Implementation

```python
def longest_substring_with_k_distinct(s, k):
    char_count = {}
    max_length = 0
    start = 0

    for end in range(len(s)):
        char_count[s[end]] = char_count.get(s[end], 0) + 1

        while len(char_count) > k:
            char_count[s[start]] -= 1
            if char_count[s[start]] == 0:
                del char_count[s[start]]
            start += 1

        max_length = max(max_length, end - start + 1)

    return max_length

# Example usage
s = "araaci"
k = 2
print(longest_substring_with_k_distinct(s, k))  # Output: 4
```

### C# Implementation

```csharp
using System;
using System.Collections.Generic;

class SlidingWindow
{
    public static int LongestSubstringWithKDistinct(string s, int k)
    {
        var charCount = new Dictionary<char, int>();
        int maxLength = 0, start = 0;

        for (int end = 0; end < s.Length; end++)
        {
            if (!charCount.ContainsKey(s[end]))
                charCount[s[end]] = 0;
            charCount[s[end]]++;

            while (charCount.Count > k)
            {
                charCount[s[start]]--;
                if (charCount[s[start]] == 0)
                    charCount.Remove(s[start]);
                start++;
            }

            maxLength = Math.Max(maxLength, end - start + 1);
        }

        return maxLength;
    }

    static void Main()
    {
        string s = "araaci";
        int k = 2;
        Console.WriteLine(LongestSubstringWithKDistinct(s, k));  // Output: 4
    }
}
```

## Conclusion

The Sliding Window technique is a versatile and efficient algorithmic approach that simplifies many problems. By maintaining and dynamically adjusting a subset of elements, you can significantly improve the performance of your solutions.