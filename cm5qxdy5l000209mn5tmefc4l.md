---
title: "Two Pointers Technique"
datePublished: Fri Jan 10 2025 15:42:00 GMT+0000 (Coordinated Universal Time)
cuid: cm5qxdy5l000209mn5tmefc4l
slug: two-pointers-technique
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736522607513/49c438f1-68a2-41b8-b498-ea70b289e9d7.jpeg
tags: csharp, algorithms, python, problem-solving, problem-solving-skills

---

### Two Pointers Technique: A Guide with Examples

The **Two Pointers Technique** is a fundamental algorithmic approach used to solve problems that involve arrays or strings efficiently. It leverages the use of two pointers that traverse the data structure in a specific manner—either from both ends toward the center or in the same direction. This technique is particularly powerful for optimizing problems with linear complexity.

### Why Use Two Pointers?

The Two Pointers Technique is useful for problems that:

* Involve **sorting** or **searching** in arrays.
    
* Require **minimizing time complexity** (e.g., from O(n^2) to O(n)).
    
* Handle **comparison-based logic**, such as finding pairs, triplets, or subarrays.
    

### Types of Two Pointers Techniques

1. **Opposite Direction Pointers**
    
    * Start pointers at both ends of the array and move them toward the center.
        
    * Commonly used for problems like checking palindromes or finding pairs with a target sum.
        
2. **Same Direction Pointers**
    
    * Start both pointers at one end and move them in the same direction.
        
    * Useful for problems involving subarray sums or sliding windows.
        

### Example 1: Finding a Pair with a Target Sum

Given a sorted array and a target sum, find if any two numbers sum to the target.

#### Problem

Input: `arr = [1, 2, 3, 4, 6]`, `target = 6`  
Output: `True` (since 2+4=62 + 4 = 62+4=6).

#### Python Solution

```python
def has_pair_with_sum(arr, target):
    left, right = 0, len(arr) - 1
    
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return True
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    
    return False

# Example Usage
arr = [1, 2, 3, 4, 6]
target = 6
print(has_pair_with_sum(arr, target))  # Output: True
```

#### C# Solution

```csharp
using System;

class Program
{
    static bool HasPairWithSum(int[] arr, int target)
    {
        int left = 0, right = arr.Length - 1;

        while (left < right)
        {
            int currentSum = arr[left] + arr[right];
            if (currentSum == target)
                return true;
            else if (currentSum < target)
                left++;
            else
                right--;
        }

        return false;
    }

    static void Main()
    {
        int[] arr = { 1, 2, 3, 4, 6 };
        int target = 6;
        Console.WriteLine(HasPairWithSum(arr, target));  // Output: True
    }
}
```

### Example 2: Removing Duplicates from a Sorted Array

Remove duplicates in-place from a sorted array and return the new length of the array.

#### Problem

Input: `arr = [2, 3, 3, 3, 6, 9, 9]`  
Output: `5` (Unique elements: `[2, 3, 6, 9]`).

#### Python Solution

```python
def remove_duplicates(arr):
    if not arr:
        return 0

    unique_index = 1  # Pointer for the position of the next unique element

    for i in range(1, len(arr)):
        if arr[i] != arr[i - 1]:
            arr[unique_index] = arr[i]
            unique_index += 1

    return unique_index

# Example Usage
arr = [2, 3, 3, 3, 6, 9, 9]
length = remove_duplicates(arr)
print(length)  # Output: 5
print(arr[:length])  # Output: [2, 3, 6, 9]
```

#### C# Solution

```csharp
using System;

class Program
{
    static int RemoveDuplicates(int[] arr)
    {
        if (arr.Length == 0) return 0;

        int uniqueIndex = 1;

        for (int i = 1; i < arr.Length; i++)
        {
            if (arr[i] != arr[i - 1])
            {
                arr[uniqueIndex] = arr[i];
                uniqueIndex++;
            }
        }

        return uniqueIndex;
    }

    static void Main()
    {
        int[] arr = { 2, 3, 3, 3, 6, 9, 9 };
        int length = RemoveDuplicates(arr);
        Console.WriteLine(length);  // Output: 5
        Console.WriteLine(string.Join(", ", arr[..length]));  // Output: 2, 3, 6, 9
    }
}
```

### Example 3: Finding Triplets with Zero Sum

Find all unique triplets in an array that sum to zero.

#### Problem

Input: `arr = [-3, -1, 0, 1, 2, -1, -4]`  
Output: `[[-1, -1, 2], [-1, 0, 1]]`.

#### Python Solution

```python
def three_sum(arr):
    arr.sort()  # Sort the array first
    result = []

    for i in range(len(arr) - 2):
        if i > 0 and arr[i] == arr[i - 1]:  # Skip duplicates
            continue
        
        left, right = i + 1, len(arr) - 1
        while left < right:
            total = arr[i] + arr[left] + arr[right]
            if total == 0:
                result.append([arr[i], arr[left], arr[right]])
                left += 1
                right -= 1
                while left < right and arr[left] == arr[left - 1]:
                    left += 1
                while left < right and arr[right] == arr[right + 1]:
                    right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1

    return result

# Example Usage
arr = [-3, -1, 0, 1, 2, -1, -4]
print(three_sum(arr))  # Output: [[-1, -1, 2], [-1, 0, 1]]
```

#### C# Solution

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static List<List<int>> ThreeSum(int[] arr)
    {
        Array.Sort(arr);
        List<List<int>> result = new();

        for (int i = 0; i < arr.Length - 2; i++)
        {
            if (i > 0 && arr[i] == arr[i - 1]) continue;

            int left = i + 1, right = arr.Length - 1;

            while (left < right)
            {
                int total = arr[i] + arr[left] + arr[right];
                if (total == 0)
                {
                    result.Add(new List<int> { arr[i], arr[left], arr[right] });
                    left++;
                    right--;
                    while (left < right && arr[left] == arr[left - 1]) left++;
                    while (left < right && arr[right] == arr[right + 1]) right--;
                }
                else if (total < 0)
                {
                    left++;
                }
                else
                {
                    right--;
                }
            }
        }

        return result;
    }

    static void Main()
    {
        int[] arr = { -3, -1, 0, 1, 2, -1, -4 };
        var triplets = ThreeSum(arr);

        foreach (var triplet in triplets)
        {
            Console.WriteLine($"[{string.Join(", ", triplet)}]");
        }
    }
}
```