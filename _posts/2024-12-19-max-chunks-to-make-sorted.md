---
title: 769. Max Chunks To Make Sorted
author: aaryaveer
date: 2024-12-19 13:20:00 -0700
# put two categories, first main-category, second sub-category
categories: [DSA, 1. Problems]
tags: [Array, Stack, Greedy, Sorting, Monotonic Stack]
---

## [[Problem](https://leetcode.com/problems/max-chunks-to-make-sorted/description/)]

 <!-- ![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge)  -->
![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)  
<!-- ![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge) -->

[**_`Array`_**](https://akr2803.github.io/tags/array/) [**_`Stack`_**](https://akr2803.github.io/tags/stack/) [**_`Greedy`_**](https://akr2803.github.io/tags/greedy/) [**_`Sorting`_**](https://akr2803.github.io/tags/sorting/) [**_`Monotonic Stack`_**](https://akr2803.github.io/tags/monotonic-stack/)

---

## Intuition  
- The key observation is that a chunk can end at index `i` if the maximum value encountered so far (`maxSoFar`) is equal to `i`. 
- This ensures all values in the chunk are valid for their positions in the sorted array.  

## Approach  

- `maxSoFar`: Tracks the maximum value seen so far in the array.  
- `cnt`: Counter to store the number of valid chunks.  

- For each index `i`:  
    - Update `maxSoFar` as the maximum of `maxSoFar` and `arr[i]`.  
    - Check if `maxSoFar` equals `i`. If true, increment the chunk counter (`cnt`).  

- Return `cnt`

### Complexity Analysis
- **Time Complexity: _O(n)_**  

- **Space Complexity: _O(1)_**

### Reference Image

| Logic behind the approach                                             | 
|--------------------------------------------------------------------------------------| 
| <img src="../assets/img/leetcode/12-19-2024-max-chunks-to-make-sorted.jpg" width=500 alt="max-chunks-to-make-sorted"/> |

#### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/December/code/MaxChunksToMakeSorted.java)
```java
class Solution { 
    public int maxChunksToSorted(int[] arr) {
        int n = arr.length;
        int maxSoFar = 0;
        int cnt = 0;

        for(int i = 0; i < n; i++){
            maxSoFar = Math.max(maxSoFar, arr[i]);

            if(maxSoFar == i){
                cnt += 1;
            }
        }
        return cnt;
    }
}
```