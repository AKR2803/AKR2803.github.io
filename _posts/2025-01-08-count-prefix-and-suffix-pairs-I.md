---
title: 3042. Count Prefix and Suffix Pairs I
author: aaryaveer
date: 2025-01-08 10:01:00 -0700
categories: [DSA, Leetcode]
tags: [Array, String, Trie, Rolling Hash, String Matching, Hash Function]
---

## [[Problem](https://leetcode.com/problems/count-prefix-and-suffix-pairs-i/description/)]

![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge) 
<!-- ![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)   -->
<!-- ![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge) -->

[**_`Array`_**](https://akr2803.github.io/tags/array/) [**_`String`_**](https://akr2803.github.io/tags/string/) [**_`Trie`_**](https://akr2803.github.io/tags/trie/) [**_`Rolling Hash`_**](https://akr2803.github.io/tags/rolling-hash/) [**_`String Matching`_**](https://akr2803.github.io/tags/string-matching/) [**_`Hash Function`_**](https://akr2803.github.io/tags/hash-function/)

---

## Intuition
- Brute Force works because constraints are small.

```
Constraints:
1 <= words.length <= 50
1 <= words[i].length <= 10
```

## Approach

- For each word, compare it with the remaining words (avoiding duplicate checks).
- Check if the first word (`str1`) is both a prefix and suffix of the second word (`str2`).

### Complexity Analysis
- **Time Complexity: _O(n^2*k)_**
  - `n`: Number of words in the array.
  - `k`: Average length of the words (checking `startsWith` and `endsWith` takes O(k)).
  
- **Space Complexity: _O(1)_**
  - No extra space used.

#### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/January/code/CountPrefixAndSuffixPairsI.java)

```java
class Solution {
    public int countPrefixSuffixPairs(String[] words) {
        int wLen = words.length;
        int cnt = 0;
        
        // brute force all pairs
        for (int i = 0; i < wLen; i++) {
            String str1 = words[i];
            
            for (int j = i + 1; j < wLen; j++) {
                String str2 = words[j];

                // Ensure`str1` is smaller or equal in length
                if (str1.length() <= str2.length()) {
                    // Check if `str1` is both a prefix and suffix of `str2`
                    if (str2.startsWith(str1) && str2.endsWith(str1)) {
                        cnt++;
                    }
                }
            }
        }

        // total count of valid pairs
        return cnt;
    }
}
```