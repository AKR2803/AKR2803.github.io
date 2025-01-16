---
title: 1639. Number of Ways to Form a Target String Given a Dictionary
author: aaryaveer
date: 2024-12-29 21:08:00 -0700
categories: [DSA, Leetcode]
tags: [Array, String, Dynamic Programming]
---

## [[Problem](https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary/description/)]

 <!-- ![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge)  -->
<!-- ![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)   -->
![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge)

[**_`Array`_**](https://akr2803.github.io/tags/array/) [**_`String`_**](https://akr2803.github.io/tags/string/) [**_`Dynamic Programming`_**](https://akr2803.github.io/tags/dynamic-programming/)

---

## Intuition
- Each character of `target` must match a character in the same position across one of the strings in `words`.
- Once a character is used at an index, no characters from the same or earlier indices can be used again.
- This problem can be approached using dynamic programming to count the number of ways to form `target` while adhering to the constraints.

---

## Approach

- Create a table to store the frequency of each character at every index of `words`.
- Define `dp[i][j]` where:
    - `i` is the index in the `target`.
    - `j` is the index in `words`.
    - `dp[i][j]` represents the number of ways to form the first `i+1` characters of `target` using up to the `j+1` characters of `words`.
- Base case: For `i=0`, initialize the values using the frequency of the first character of `target`.
- Transition: Update `dp[i][j]` based on the previous state `dp[i-1][k]` and the frequency table.

- The result is the sum of all valid ways to form the complete `target`.
- Since the result can be very large, return the value modulo `10^9 + 7`.

---

### Complexity Analysis

- **Time Complexity: _O(wordLength * numWords + targetLength * wordLength^2)_**
    - Character Frequency Table: `O(wordLength * numWords)`  
    - Loop through each character in all words to build the table.
    - DP: `O(targetLength * wordLength^2)`  
    - For each character in the `target`, compute transitions between all valid indices.
S
### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/December/code/FormATargetStringGivenADictionary.java)

```java
class Solution {
    public int numWays(String[] words, String target) {
        final int MOD = 1_000_000_007; // Modulo for the result to prevent overflow
        int numWords = words.length;
        int wordLength = words[0].length();
        int targetLength = target.length();

        // Edge case: if the length of a word is less than the target, impossible to form
        if (wordLength < targetLength) return 0;

        // `charFrequency[i][c]` stores the frequency of character `c` at index `i` in all words
        int[][] charFrequency = new int[wordLength][26];
        for (int i = 0; i < wordLength; ++i) {
            for (int j = 0; j < numWords; ++j) {
                charFrequency[i][words[j].charAt(i) - 'a'] += 1;
            }
        }

        // `dp[i][j]` represents the number of ways to form the first `i+1` characters of target
        // using the first `j+1` indices of the words
        int[][] dp = new int[targetLength][wordLength];

        for (int i = 0; i < targetLength; ++i) {
            int targetCharIndex = target.charAt(i) - 'a';
            for (int j = i; j < wordLength - (targetLength - 1 - i); ++j) { // Ensure valid remaining indices
                if (i == 0) {
                    // For the first character of the target, directly take the frequency
                    dp[i][j] = charFrequency[j][targetCharIndex];
                } else {
                    long sum = 0;
                    for (int k = i - 1; k < j; ++k) {
                        sum += dp[i - 1][k];
                    }
                    dp[i][j] = (int) ((sum * charFrequency[j][targetCharIndex]) % MOD);
                }
            }
        }

        // Calculate the final answer by summing up all valid ways to form the entire target
        int result = 0;
        for (int i = targetLength - 1; i < wordLength; ++i) {
            result = (result + dp[targetLength - 1][i]) % MOD;
        }

        return result;
    }
}
```