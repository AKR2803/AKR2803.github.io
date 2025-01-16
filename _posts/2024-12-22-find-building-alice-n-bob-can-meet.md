---
title: 2940. Find Building Where Alice and Bob Can Meet
author: aaryaveer
date: 2024-12-22 16:25:00 -0700
# put two categories, first main-category, second sub-category
categories: [DSA, Leetcode]
tags: [Array, Binary Search, Stack, Binary Indexed Tree, Segment Tree, Heap, Priority Queue, Monotonic Stack]
---

## [[Problem](https://leetcode.com/problems/find-building-where-alice-and-bob-can-meet/description/)]

<!-- ![Easy](https://img.shields.io/badge/Easy-green?style=for-the-badge)  -->
<!-- ![Medium](https://img.shields.io/badge/Medium-yellow?style=for-the-badge)   -->
![Hard](https://img.shields.io/badge/Hard-red?style=for-the-badge)

[**_`Array`_**](https://akr2803.github.io/tags/array/) [**_`Binary Search`_**](https://akr2803.github.io/tags/binary-search/) [**_`Stack`_**](https://akr2803.github.io/tags/stack/) [**_`Binary Indexed Tree`_**](https://akr2803.github.io/tags/binary-indexed-tree/) [**_`Segment Tree`_**](https://akr2803.github.io/tags/segment-tree/) [**_`Heap (Priority Queue)`_**](https://akr2803.github.io/tags/heap/) [**_`Monotonic Stack`_**](https://akr2803.github.io/tags/monotonic-stack/)

---

## Intuition
- The problem revolves around determining the leftmost building where Alice and Bob can meet, based on specific movement constraints (i.e., moving to a taller building). 
- Since Alice and Bob may not always be able to move to a common building directly, a deferred query processing can be used. 
- By leveraging height constraints and maintaining pending queries, we can efficiently compute the result.

## Approach
- Deferred Queries
    - Use an array of lists (`lists`) to store pending queries for each building where Alice or Bob cannot directly move to the target building.
   - Populate the `lists` during the initial traversal of queries.

- Immediate Meeting
    - If Alice and Bob can immediately meet based on their starting buildings and the height conditions, record the result in the `res` array.

- Priority Queue
   - Utilize a priority queue to handle deferred queries efficiently. 
   - Each query in the queue is processed when the height of the current building allows it.

- Finally iterate over each building, updating results for pending queries where the height condition is now satisfied.

### Complexity Analysis
- **Time Complexity: _O(n + qlog q)_**:
  - Initial query processing: `O(q)`, where `q` is the no. of queries.
  - Processing deferred queries: `O(n+qlog q)`, where `n` is the no. of buildings & `O(qlogq)` for priority queue operations.

- **Space Complexity: _O(n + q)_**:
  - Storage for `lists`: `O(n+q)` to store all pending queries.
  - Priority queue: `O(q)`

#### [Code](https://github.com/AKR-2803/DSA-Declassified/blob/main/POTD-Leetcode/December/code/FindBuildingAliceNBobCanMeet.java)
```java
class Solution {
    public int[] leftmostBuildingQueries(int[] heights, int[][] queries) {
        //Array of lists to store pending queries for each building
        List<int[]>[] lists = new ArrayList[heights.length];
        
        // Initialize the result array with -1 (meaning no meeting point found)
        int[] res = new int[queries.length];
        Arrays.fill(res, -1);
        
        for (int i = 0; i < queries.length; i++) {
            int aIndex = queries[i][0];  // Alice starting building index
            int bIndex = queries[i][1];  // Bob starting building index

            // Case 1: Alice starts to the right of Bob
            if (aIndex > bIndex) {
                // If Alice can directly move to Bob's building
                if (heights[aIndex] > heights[bIndex]) {
                    res[i] = aIndex;
                } else {
                    // Otherwise, store the query for processing later
                    if (lists[aIndex] == null)
                        lists[aIndex] = new ArrayList<>();
                    lists[aIndex].add(new int[] { heights[bIndex], i });
                }
            }
            // Case 2: Bob starts to the right of Alice
            else if (aIndex < bIndex) {
                // If Bob can directly move to Alice's building
                if (heights[aIndex] < heights[bIndex]) {
                    res[i] = bIndex;
                } else {
                    // Otherwise, store the query for processing later
                    if (lists[bIndex] == null)
                        lists[bIndex] = new ArrayList<>();
                    lists[bIndex].add(new int[] { heights[aIndex], i });
                }
            }
            // Case 3: Both start at the same building
            else {
                res[i] = aIndex;
            }
        }
        
        // priority queue(minHeap) to handle pending queries sorted by heights
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        
        // Iterate over each building to process pending queries
        for (int i = 0; i < heights.length; i++) {
            // Process queries in the priority queue where height is less than the current building's height
            while (!minHeap.isEmpty() && minHeap.peek()[0] < heights[i]) {
                res[minHeap.poll()[1]] = i;
            }
            
            // Add pending queries for the current building to the priority queue
            if (lists[i] != null) {
                for (int[] arr : lists[i]) {
                    minHeap.add(arr);
                }
            }
        }
        
        return res;
    }
}
```