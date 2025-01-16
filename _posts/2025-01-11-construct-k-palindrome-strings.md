---
title: 1400. Construct K Palindrome Strings
author: aaryaveer
date: 2025-01-11 11:19:00 -0700
categories: [DSA, Leetcode]
tags: [Hash Table, String, Greedy, Counting]
---

## [[Problem](https://leetcode.com/problems/construct-k-palindrome-strings/description/)]

<!-- ![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge)  -->
![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)  
<!-- ![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge) -->

[**_`Hash Table`_**](https://akr2803.github.io/tags/hash-table/) [**_`String`_**](https://akr2803.github.io/tags/string/) [**_`Greedy`_**](https://akr2803.github.io/tags/greedy/) [**_`Counting`_**](https://akr2803.github.io/tags/counting/)

---

## Intuition

- A palindrome reads the same forward and backward.

- `Even Length Palindrome` : If a string has an even length, all characters must appear an even no. of times. E.g. `abba`, `abaaba`, `aabbaa` etc.

- `Odd Length Palindrome`: If a string has an odd length, all but one character must appear an even no. of times (the character that appears an odd no. of times would be the **middle character in the palindrome**). E.g. "aca", "abcba", "abbebba", "accca", "cacac". 

- Observe the palindromes and how can we break them down

```
CASE-1: EVEN LENGTH

Consider s = "aabb", try to rearrange this to form a palindrom => "abba"

- for k=1 => true => "aabb"
- for k=2 => true => "aa", "bb"
- for k=3 => true => "a", "a", "bb"
- for k=4 => true => "a", "a", "b", "b"

------------------------------------------------

CASE-2: ODD LENGTH

Consider s = "aabbcccde", try to rearrange to form a palindrome => "abcdcba"

- for k=1 => false => no `single` string can be made a palindrome  
- for k=2 => false => can't make 2 palindromic strings
- for k=3 => true  => "ada", "beb", "ccc"   (focus on this case)
- for k=4 => true  => "ada", "beb", "cc", "c"
- for k=5 => true  => "ada", "beb", "c", "c", "c"
- for k=6 => false
- for k=7 => false
- for k=8 => false
- for k=9 => true  => "a", "a", "b", "b", "d", "e", "c", "c", "c"


- Observe `k=3` very carefully and understand what happened

- k=3 wanted 3 palindromes:  ____ , ____,  ____ 

- We have "aabbcccde", lets try to make these 3 palindromes

- 3 of them can be =>  "a_a", "b_b", "ccc"
                         ^      ^
                         |      |
- Adjust "d" & "e"-------d------e     (characters with odd frequencies can be placed in the middle of each palindrome)

- 3 palindromes => "ada", "beb", "ccc"
```

- **Observation**: If the total no. of characters with odd frequencies is `<= k`, we can accomodate these odd frequency characters!

## Approach

- Straightforward approach, calculate total characters in string `s` with odd frequencies.
- Return `k >= oddFrequencies`

### Complexity Analysis
- **Time Complexity: _O(n)_**
    - `n`: string length.
- **Space Complexity: _O(1)_**

#### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/January/code/ConstructKPalindromeStrings.java)

```java
class Solution {
    public boolean canConstruct(String s, int k) {
        if(k > s.length()){
            return false;
        }
        
        // single character is always a palindrome
        if(k == s.length()){
            return true;
        }

        int[] freq = new int[26];
        
        // update frequency of each character
        for(char ch : s.toCharArray()){
            freq[ch - 'a'] += 1;
        }

        int oddFrequencies = 0;

        // find no. of characters with odd frequencies
        for(int i = 0; i < freq.length; i++){
            if(freq[i] % 2 != 0){
                oddFrequencies += 1;
            }
        }

        return k >= oddFrequencies;
    }
}
```