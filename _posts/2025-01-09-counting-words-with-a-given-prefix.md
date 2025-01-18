---
title: 2185. Counting Words With a Given Prefix
author: aaryaveer
date: 2025-01-09 09:32:00 -0700
categories: [DSA, 1. Problems]
tags: [Array, String, String Matching]
---

## [[Problem](https://leetcode.com/problems/counting-words-with-a-given-prefix/description/)]

![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge) 
<!-- ![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)   -->
<!-- ![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge) -->

[**_`Array`_**](https://akr2803.github.io/tags/array/) [**_`String`_**](https://akr2803.github.io/tags/string/) [**_`String Matching`_**](https://akr2803.github.io/tags/string-matching/)

---

## Intuition
- Brute Force works because constraints are small.

```
Constraints
1 <= words.length <= 100
1 <= words[i].length, pref.length <= 100
```

## Approach

- For each word in `words` check if it has `pref` as prefix prefix.

### Complexity Analysis
- **Time Complexity: _O(n)_**
  - `n`: size of `words` array.
  
- **Space Complexity: _O(1)_**
  - No extra space used.

#### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/January/code/CountingWordsWithAGivenPrefix.java)

```java
class Solution {
    public int prefixCount(String[] words, String pref) {
        int cnt = 0;
        for(String str : words){
            // `str` contains `pref` as prefix 
            if(str.startsWith(pref)){
                cnt += 1; // increment cnt
            }
        }
        return cnt;
    }
}
```