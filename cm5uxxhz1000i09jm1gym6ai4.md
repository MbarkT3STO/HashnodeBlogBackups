---
title: "Kadane's Algorithm"
datePublished: Mon Jan 13 2025 11:08:17 GMT+0000 (Coordinated Universal Time)
cuid: cm5uxxhz1000i09jm1gym6ai4
slug: kadanes-algorithm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736766475853/6dbb1ec1-53b1-4574-b106-2b1d62e551ba.jpeg
tags: csharp, algorithms, python

---

Finding the maximum sum of a subarray in a given array is a classic problem in computer science and is efficiently solved using **Kadane's Algorithm**. This algorithm is celebrated for its simplicity and optimal time complexity of O(n)O(n)O(n), making it a go-to solution for this problem.

In this blog post, we’ll delve into the theory behind Kadane's Algorithm, implement it in both Python and C#, and examine its practical applications.

## What is Kadane's Algorithm?

Kadane's Algorithm is used to find the **maximum sum of a contiguous subarray** within a one-dimensional array of numbers. The key idea is to use dynamic programming to track the maximum sum ending at each position while updating the global maximum.

## How Does It Work?

The algorithm maintains two variables:

1. `current_max`: The maximum sum of the subarray ending at the current position.
    
2. `global_max`: The maximum sum of any subarray encountered so far.
    

For each element in the array:

* Update `current_max` as the maximum of the current element itself or the sum of `current_max` and the current element.
    
* Update `global_max` if `current_max` exceeds it.
    

## Steps of Kadane's Algorithm

1. Initialize `current_max` and `global_max` with the first element of the array.
    
2. Iterate through the array starting from the second element.
    
3. For each element:
    
    * Update `current_max` as `max(current_max + element, element)`.
        
    * Update `global_max` as `max(global_max, current_max)`.
        
4. At the end of the iteration, `global_max` contains the maximum sum of any contiguous subarray.
    

## Python Implementation

Here’s how Kadane's Algorithm is implemented in Python:

```python
def max_subarray_sum(nums):
    if not nums:
        return 0  # Handle edge case for empty array
    
    current_max = global_max = nums[0]
    
    for num in nums[1:]:
        current_max = max(num, current_max + num)
        global_max = max(global_max, current_max)
    
    return global_max

# Example Usage
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_sum(nums)
print(f"The maximum subarray sum is: {result}")
```

**Output:**

```plaintext
The maximum subarray sum is: 6
```

Here, the maximum subarray is `[4, -1, 2, 1]`, with a sum of `6`.

## C# Implementation

Below is the equivalent implementation in C#:

```csharp
using System;

class KadaneAlgorithm
{
    public static int MaxSubarraySum(int[] nums)
    {
        if (nums.Length == 0) return 0; // Handle edge case for empty array
        
        int currentMax = nums[0];
        int globalMax = nums[0];

        for (int i = 1; i < nums.Length; i++)
        {
            currentMax = Math.Max(nums[i], currentMax + nums[i]);
            globalMax = Math.Max(globalMax, currentMax);
        }

        return globalMax;
    }

    static void Main(string[] args)
    {
        int[] nums = { -2, 1, -3, 4, -1, 2, 1, -5, 4 };
        int result = MaxSubarraySum(nums);
        Console.WriteLine($"The maximum subarray sum is: {result}");
    }
}
```

**Output:**

```plaintext
The maximum subarray sum is: 6
```

## Applications of Kadane's Algorithm

Kadane's Algorithm is not only useful in competitive programming but also finds applications in various real-world scenarios, such as:

* **Financial Analysis**: Identifying periods with maximum profit or minimum loss.
    
* **Signal Processing**: Finding the longest sequence with maximum signal strength.
    
* **Genomics**: Locating regions of DNA with high scoring patterns.
    

## Edge Cases to Consider

1. **All Negative Numbers**: The maximum sum will be the largest (least negative) number.
    
    ```python
    nums = [-3, -4, -2, -1, -5]
    # Output: -1
    ```
    
2. **Single Element Array**: The maximum sum will be the only element.
    
    ```python
    nums = [5]
    # Output: 5
    ```
    
3. **Empty Array**: Should return 0 or handle as a special case.
    

## Complexity Analysis

* **Time Complexity**: O(n)  
    Each element is visited once.
    
* **Space Complexity**: O(1)  
    Only a few variables are used for tracking.
    

## Conclusion

Kadane's Algorithm is an elegant solution for the maximum subarray sum problem, combining simplicity with efficiency. Whether you're solving competitive programming problems or analyzing financial data, this algorithm is a reliable tool in your arsenal.