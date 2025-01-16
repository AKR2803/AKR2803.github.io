---
title: 2471. Min. No. of Operations to Sort a Binary Tree by Level
author: aaryaveer
date: 2024-12-23 11:09:00 -0700
# put two categories, first main-category, second sub-category
categories: [DSA, Leetcode]
tags: [Tree, Breadth-First Search, Binary Tree]
---

##  [[Problem](https://leetcode.com/problems/minimum-number-of-operations-to-sort-a-binary-tree-by-level/description/)]

<!-- ![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge)  -->
![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)  
<!-- ![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge) -->

[**_`Tree`_**](https://akr2803.github.io/tags/tree/) [**_`Breadth-First Search`_**](https://akr2803.github.io/tags/breadth-first-search/) [**_`Binary Tree`_**](https://akr2803.github.io/tags/binary-tree/)

---

## Intuition
- Traversing level-by-level = BFS

## Approach
- Traverse using BFS:
   - Use a queue to perform a BFS of the tree.
   - For each level, store the values in an array.

- Sorting with Minimum Swaps:
   - Pass the `currentLevel` array to `findSwaps()` to find the minimum swaps required to sort them.
   - Use a hashmap to track the indices of elements and simulate swaps to bring elements to their correct positions.

- Sum the swaps required for all levels and return the result.

### Complexity Analysis
- **Time Complexity: _O(nlogn)_**
    - BFS: Visiting all nodes once `O(n)`.
    - Sorting `O(klogk)` and swapping `O(k)` per level, where `k` is the no. of nodes at a level.
    - In the worst case, summing over all levels, the dominant term is `O(nlogk)`.
    - `k` can be max `n/2` in case of complete binary tree. Hence `O(nlogk) ≈ O(nlogn)`

- **Space Complexity: _O(n)_**
    - Queue has at max `n/2` elements (Complete binary tree has maximum `n/2` elements at a level)
    - Additional space for arrays and maps within `findSwaps()`, but bounded by `O(n)`

#### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/December/code/MinNoOfOpSortBinaryTreeByLvl.java)
```java
class Solution {
    public int minimumOperations(TreeNode root) {
        int totalSwaps = 0;
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);

        // BFS
        while(!q.isEmpty()){
            int lvlSize = q.size(); // total nodes at current level

            int[] currentLevel = new int[lvlSize]; // array with node values at this level

            for(int i = 0; i < lvlSize; i++){
                TreeNode currentNode = q.poll();   // remove the current node

                currentLevel[i] = currentNode.val; // store current node's value

                if(currentNode.left != null){
                    q.add(currentNode.left);  // if present, add left child 
                }

                if(currentNode.right != null){
                    q.add(currentNode.right); // if present, add right child
                }
            }
            totalSwaps += findSwaps(currentLevel); // count swaps to sort this level
        }
        return totalSwaps; // total swaps required
    }

    // find total swaps needed to sort a particular level
    public int findSwaps(int[] original){
        int[] sortedArr = original.clone();   // sort copy of array, to know the correct index positions
        Arrays.sort(sortedArr);

        int swaps = 0;

        Map<Integer, Integer> mp = new HashMap<>(); // map to track element indices in the original array

        // store [elem : index] as key value pair in the Map
        for(int i = 0; i < original.length; i++){
            mp.put(original[i], i);
        }

        // count swaps to sort the array
        for(int i = 0; i < original.length; i++){
            if(original[i] != sortedArr[i]){    // if the current element is not in the correct position

                int pos = mp.get(sortedArr[i]); // get the correct position of the this element
                mp.put(original[i], pos);       // update the map with the swapped element's new position
                original[pos] = original[i];    // swap the elements in the original array

                swaps += 1;
            }
        }
        return swaps;
    }
}
```