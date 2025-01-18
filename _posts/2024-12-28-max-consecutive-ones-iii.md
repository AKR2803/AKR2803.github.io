---
title: 1004. Max Consecutive Ones III
author: aaryaveer
date: 2024-12-28 15:33:00 -0700
categories: [DSA, 1. Problems]
tags: [Array, Binary Search, Sliding Window, Prefix Sum]
---

## [[Problem](https://leetcode.com/problems/max-consecutive-ones-iii/description/)]

 <!-- ![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge)  -->
![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)  
<!-- ![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge) -->

[**_`Array`_**](https://akr2803.github.io/tags/array/) [**_`Binary Search`_**](https://akr2803.github.io/tags/binary-search/) [**_`Sliding Window`_**](https://akr2803.github.io/tags/sliding-window/) [**_`Prefix Sum`_**](https://akr2803.github.io/tags/prefix-sum/)

---
## Intuition

- Interpret the question as: **"Longest subarray with at most `k` zeroes"**. 
- There is **NO ACTUAL FLIPPING** you need to do. 
- The question is reduced to finding the maxImum subarray length with at most `k` zeroes.

---

## Approach 1. Sliding Window

- Use a sliding window defined by `left` and `right` pointers.
- Increment the `right` pointer while counting zeroes in the window.
- If the count of zeroes exceeds `k`, increment the `left` pointer to reduce the window size until the zero count is valid.
- Update the maximum window size during the process.

### Complexity Analysis

- **Time Complexity: _O(n)_**  
    - Each element is processed at most twice (once by `right` and once by `left`).
- **Space Complexity: _O(1)_**  
    - No extra space used.


```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        
        // start left from index `0`, and initialize maxLen to `0`
        // `maxLen` is the length of longest window where the no. of zeroes <= k
        int left = 0, maxLen = 0;

        // iterate with the right pointer to "expand" the window
        for (int right = 0; right < n; right++) {
            // if a `0` is encountered, decrement `k`, which tracks the no. of zeroes allowed in the window
            if (nums[right] == 0) {
                k--;
            }

            // if the no. of zeroes in the window exceeds `k`, move the `left` pointer to shrink the window
            // This ensures we always maintain at most `k` zeroes in the window
            while (k < 0) {
                // if the element at the `left` pointer is 0, increment `k` to "restore" a zero
                if (nums[left] == 0) {
                    k++;
                }
                // move the `left` pointer forward to shrink the window
                left++;
            }

            // update `maxLen` with the maximum window length encountered so far
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }
}
```

---

## Approach 2. Prefix Sum 

Because the array contains only `0s` and `1s`, the sum in a given range `[left, right]` represents the count of `1s` in that window.

- For example, if the sum is `4`, it means there are `4` ones in that window, and the rest are zeros.
- The number of zeroes can be calculated as:  
  **`No. of zeroes = window_length - No. of ones`**

- Now, to solve the problem:  
    - Increment the `right` pointer to expand the window, allowing up to `k` zeroes in the window. Continue expanding until there are at most `k` zeroes.  
    - If the number of zeroes exceeds `k`, start incrementing the `left` pointer.  
    - Incrementing `left` effectively shifts the window. Initially, only `right` was expanding the window length, but by moving `left`, the window size becomes constant while adjusting the zero count.  
    - Continue this process until the number of zeroes in the window becomes less than or equal to `k`.


### Complexity Analysis

- **Time Complexity: _O(n)_**  
  One pass to calculate the prefix sum, and one pass to adjust the window.
- **Space Complexity: _O(n)_**  
  Additional space for the prefix sum array.


### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/December/code/BestSightseeingPair.java)

```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        
        // initialize the prefixSum array to store cumulative sums
        // prefixSum[i] will represent the sum of elements in nums[0] to nums[i-1]
        int[] prefixSum = new int[n + 1];

        // compute the prefix sum array
        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }

        // start left from index `0`, and initialize maxLen to `0`
        // `maxLen` is the length of longest window where the no. of zeroes <= k
        int left = 0, maxLen = 0;

        // iterate with the right pointer to "expand" the window
        for (int right = 0; right < n; right++) {
            // calculate the no. of 1s in the current window [left, right]
            int numOnes = prefixSum[right + 1] - prefixSum[left];

            // calculate the no. of 0s in the current window
            // remember, `no. of zeros = window_length - no. of ones`
            int numZeroes = (right - left + 1) - numOnes;

            // if the no. of zeroes exceeds k, increment `left`
            // this will shrink the window to make the no. of zeroes <= k
            if (numZeroes > k) {
                left++;
            } else {
                // if the window is valid, update `maxLen`
                maxLen = Math.max(maxLen, right - left + 1);
            }
        }
        return maxLen;
    }
}
```