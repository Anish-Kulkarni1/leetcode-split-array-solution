# 410. Split Array Largest Sum (Leetcode Hard)

This repository contains my solution and detailed thought process for the Leetcode problem [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/).

---

## My Thought Process & Approach

The problem asks us to **minimize** the **largest** sum of any subarray, given a fixed number of splits (`k`). This "minimize the maximum" or "maximize the minimum" structure is a classic signal that the problem can be solved using **Binary Search on the Answer**.

1.  **Define the Search Space:**
    * What is the smallest possible value for the "largest sum"? It has to be at least the largest single number in the array. If we don't allow a sum that large, we can't even place that number in any subarray. So, `low = max(nums)`.
    * What is the largest possible value? It's the sum of the entire array, which happens when we have only one split (`k=1`). So, `high = sum(nums)`.
    * Our answer must lie somewhere in the range `[low, high]`.

2.  **Binary Search Logic:**
    * We can now binary search within this range. For any `mid` value we pick, we need to ask a crucial question: **"Is it possible to split the array into `k` or fewer subarrays such that the sum of each subarray does not exceed `mid`?"**
    * To answer this, I wrote a helper logic (the inner loop in my code). I greedily create subarrays, adding numbers to the current subarray until the sum exceeds `mid`. When it does, I increment my subarray count and start a new subarray with the current number.

3.  **Adjusting the Search Space:**
    * If the number of subarrays required (`count`) is **less than or equal to `k`**, it means that `mid` is a valid (or even too large) potential answer. We can try to find an even smaller "largest sum," so we set `high = mid`.
    * If the number of subarrays required is **greater than `k`**, it means our `mid` value was too small and restrictive. We need to allow for a larger subarray sum, so we set `low = mid + 1`.

4.  **The Final Answer:**
    * The loop continues until `low` and `high` converge. The final value of `low` (or `end` in my code) will be the minimum possible value for the largest subarray sum.

---

## Complexity Analysis

* **Time Complexity:** $O(N \cdot \log(S))$, where $N$ is the number of elements in `nums` and $S$ is the sum of all elements in `nums`.
    * The binary search runs $O(\log(S))$ times because the search space is from `max(nums)` to `sum(nums)`.
    * For each binary search step, we iterate through the entire array once to check if the `mid` value is feasible, which takes $O(N)$ time.
* **Space Complexity:** $O(1)$, as I am only using a few variables to store the sums and counts, not any extra data structures that scale with the input size.
