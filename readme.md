# 🎯 Complete Array Interview Sheet — C++

> **The ultimate array preparation guide for coding interviews & tests.**
> Covers Easy → Medium → Hard. Brute → Better → Optimal. Every must-know problem.

---

## 📑 Table of Contents

**EASY (1–18)**
1. Largest Element in Array
2. Second Largest Element (without sorting)
3. Check if Array is Sorted
4. Remove Duplicates from Sorted Array
5. Left Rotate Array by 1
6. Left Rotate Array by K places
7. Right Rotate Array by K places
8. Move Zeros to End
9. Linear Search
10. Union of Two Sorted Arrays
11. Intersection of Two Sorted Arrays
12. Find Missing Number (1 to N)
13. Maximum Consecutive Ones
14. Find Single Number (others appear twice)
15. Longest Subarray with Sum K (positives)
16. Longest Subarray with Sum K (positives + negatives)
17. Reverse an Array
18. Find Min and Max in Array

**MEDIUM (19–40)**
19. Two Sum Problem
20. Sort an Array of 0s, 1s, 2s (Dutch National Flag)
21. Majority Element (> N/2 times) — Boyer-Moore
22. Maximum Subarray Sum — Kadane's Algorithm
23. Print Subarray with Maximum Sum
24. Best Time to Buy and Sell Stock
25. Rearrange Array by Sign
26. Next Permutation
27. Leaders in an Array
28. Longest Consecutive Sequence
29. Set Matrix Zeros
30. Rotate Matrix by 90 degrees
31. Print Matrix in Spiral Order
32. Count Subarrays with Sum K
33. Pascal's Triangle
34. Majority Element (> N/3 times)
35. 3 Sum Problem
36. 4 Sum Problem
37. Largest Subarray with 0 Sum
38. Count Subarrays with XOR K
39. Merge Overlapping Intervals
40. Merge Two Sorted Arrays without Extra Space

**HARD (41–55)**
41. Find Repeating and Missing Number
42. Count Inversions in Array
43. Reverse Pairs
44. Maximum Product Subarray
45. Trapping Rain Water
46. Container with Most Water
47. Median of Two Sorted Arrays
48. Kth Largest / Smallest Element
49. Search in Rotated Sorted Array
50. Find Minimum in Rotated Sorted Array
51. Single Element in Sorted Array (others twice)
52. Find Peak Element
53. Allocate Books / Capacity to Ship Packages
54. Aggressive Cows / Painter's Partition
55. Median in Row-wise Sorted Matrix

---

## ⚡ Time/Space Complexity Cheat Sheet

| Approach | Time | When to Use |
|----------|------|-------------|
| Brute Force (nested loops) | O(N²) | Always start here |
| Hashing | O(N) avg | Lookup / counting |
| Two Pointer | O(N) | Sorted arrays, pairs |
| Sliding Window | O(N) | Subarray problems |
| Binary Search | O(log N) | Sorted / search space |
| Prefix Sum | O(N) | Range queries |
| Kadane / DP | O(N) | Optimization problems |

---

# 🟢 EASY PROBLEMS

---

## 1. Largest Element in Array

**Problem:** Find the largest element in the array.

```cpp
#include <bits/stdc++.h>
using namespace std;

int largestElement(vector<int>& arr) {
    int largest = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        if (arr[i] > largest) largest = arr[i];
    }
    return largest;
}
// TC: O(N) | SC: O(1)
```

---

## 2. Second Largest Element (Without Sorting)

**Problem:** Find the second largest distinct element.

```cpp
int secondLargest(vector<int>& arr) {
    int largest = INT_MIN, second = INT_MIN;
    for (int x : arr) {
        if (x > largest) {
            second = largest;
            largest = x;
        } else if (x > second && x != largest) {
            second = x;
        }
    }
    return (second == INT_MIN) ? -1 : second;
}
// TC: O(N) single pass | SC: O(1)
```

**Second Smallest** — same idea: track `smallest` and `second_smallest`.

---

## 3. Check if Array is Sorted

```cpp
// Sorted ascending (non-decreasing)
bool isSorted(vector<int>& arr) {
    for (int i = 1; i < arr.size(); i++)
        if (arr[i] < arr[i - 1]) return false;
    return true;
}

// Sorted and rotated check (LeetCode 1752)
bool isSortedAndRotated(vector<int>& arr) {
    int count = 0, n = arr.size();
    for (int i = 0; i < n; i++)
        if (arr[i] > arr[(i + 1) % n]) count++;
    return count <= 1;
}
// TC: O(N) | SC: O(1)
```

---

## 4. Remove Duplicates from Sorted Array

**Problem:** Remove duplicates in-place, return new length.

```cpp
int removeDuplicates(vector<int>& arr) {
    if (arr.empty()) return 0;
    int i = 0;
    for (int j = 1; j < arr.size(); j++) {
        if (arr[j] != arr[i]) {
            i++;
            arr[i] = arr[j];
        }
    }
    return i + 1;
}
// TC: O(N) | SC: O(1) — Two pointer technique
```

---

## 5. Left Rotate Array by 1

```cpp
void leftRotateByOne(vector<int>& arr) {
    int first = arr[0];
    for (int i = 1; i < arr.size(); i++)
        arr[i - 1] = arr[i];
    arr[arr.size() - 1] = first;
}
// TC: O(N) | SC: O(1)
```

---

## 6. Left Rotate Array by K places

**Optimal — Reversal Algorithm (no extra space):**

```cpp
void reverse(vector<int>& arr, int l, int r) {
    while (l < r) swap(arr[l++], arr[r--]);
}

void leftRotate(vector<int>& arr, int k) {
    int n = arr.size();
    k = k % n;
    reverse(arr, 0, k - 1);       // reverse first k
    reverse(arr, k, n - 1);       // reverse remaining
    reverse(arr, 0, n - 1);       // reverse whole array
}
// TC: O(N) | SC: O(1)
```

---

## 7. Right Rotate Array by K places

```cpp
void rightRotate(vector<int>& arr, int k) {
    int n = arr.size();
    k = k % n;
    reverse(arr, 0, n - 1);
    reverse(arr, 0, k - 1);
    reverse(arr, k, n - 1);
}
// TC: O(N) | SC: O(1)
```

---

## 8. Move Zeros to End

```cpp
void moveZeros(vector<int>& arr) {
    int j = -1, n = arr.size();
    // Find first zero
    for (int i = 0; i < n; i++) {
        if (arr[i] == 0) { j = i; break; }
    }
    if (j == -1) return;
    // Swap non-zeros with zero pointer
    for (int i = j + 1; i < n; i++) {
        if (arr[i] != 0) {
            swap(arr[i], arr[j]);
            j++;
        }
    }
}
// TC: O(N) | SC: O(1)
```

---

## 9. Linear Search

```cpp
int linearSearch(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++)
        if (arr[i] == target) return i;
    return -1;
}
// TC: O(N) | SC: O(1)
```

---

## 10. Union of Two Sorted Arrays

```cpp
vector<int> unionArr(vector<int>& a, vector<int>& b) {
    vector<int> result;
    int i = 0, j = 0;
    while (i < a.size() && j < b.size()) {
        if (a[i] <= b[j]) {
            if (result.empty() || result.back() != a[i]) result.push_back(a[i]);
            i++;
        } else {
            if (result.empty() || result.back() != b[j]) result.push_back(b[j]);
            j++;
        }
    }
    while (i < a.size()) {
        if (result.empty() || result.back() != a[i]) result.push_back(a[i]);
        i++;
    }
    while (j < b.size()) {
        if (result.empty() || result.back() != b[j]) result.push_back(b[j]);
        j++;
    }
    return result;
}
// TC: O(N + M) | SC: O(N + M)
```

---

## 11. Intersection of Two Sorted Arrays

```cpp
vector<int> intersection(vector<int>& a, vector<int>& b) {
    vector<int> result;
    int i = 0, j = 0;
    while (i < a.size() && j < b.size()) {
        if (a[i] < b[j]) i++;
        else if (a[i] > b[j]) j++;
        else {
            result.push_back(a[i]);
            i++; j++;
        }
    }
    return result;
}
// TC: O(N + M) | SC: O(1) extra
```

---

## 12. Find Missing Number (1 to N)

**Optimal — Sum Formula:**

```cpp
int missingNumber(vector<int>& arr, int n) {
    int total = n * (n + 1) / 2;
    int sum = 0;
    for (int x : arr) sum += x;
    return total - sum;
}
// TC: O(N) | SC: O(1)
```

**Alternative — XOR (avoids overflow):**

```cpp
int missingNumberXOR(vector<int>& arr, int n) {
    int xor1 = 0, xor2 = 0;
    for (int i = 0; i < n - 1; i++) {
        xor2 ^= arr[i];
        xor1 ^= (i + 1);
    }
    xor1 ^= n;
    return xor1 ^ xor2;
}
```

---

## 13. Maximum Consecutive Ones

```cpp
int maxConsecutiveOnes(vector<int>& arr) {
    int count = 0, maxCount = 0;
    for (int x : arr) {
        if (x == 1) {
            count++;
            maxCount = max(maxCount, count);
        } else count = 0;
    }
    return maxCount;
}
// TC: O(N) | SC: O(1)
```

---

## 14. Single Number (Others Appear Twice)

```cpp
int singleNumber(vector<int>& arr) {
    int xorSum = 0;
    for (int x : arr) xorSum ^= x;
    return xorSum;
}
// TC: O(N) | SC: O(1) — XOR cancels duplicates
```

---

## 15. Longest Subarray with Sum K (Positives Only)

**Sliding Window:**

```cpp
int longestSubarrayPositive(vector<int>& arr, int k) {
    int left = 0, sum = 0, maxLen = 0;
    for (int right = 0; right < arr.size(); right++) {
        sum += arr[right];
        while (sum > k) sum -= arr[left++];
        if (sum == k) maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
// TC: O(2N) | SC: O(1)
```

---

## 16. Longest Subarray with Sum K (Positives + Negatives)

**Hashing — Prefix Sum:**

```cpp
int longestSubarray(vector<int>& arr, int k) {
    unordered_map<int, int> prefixIdx;
    int sum = 0, maxLen = 0;
    for (int i = 0; i < arr.size(); i++) {
        sum += arr[i];
        if (sum == k) maxLen = max(maxLen, i + 1);
        if (prefixIdx.find(sum - k) != prefixIdx.end())
            maxLen = max(maxLen, i - prefixIdx[sum - k]);
        if (prefixIdx.find(sum) == prefixIdx.end())
            prefixIdx[sum] = i;
    }
    return maxLen;
}
// TC: O(N) avg | SC: O(N)
```

---

## 17. Reverse an Array

```cpp
void reverseArray(vector<int>& arr) {
    int l = 0, r = arr.size() - 1;
    while (l < r) swap(arr[l++], arr[r--]);
}
// TC: O(N) | SC: O(1)
```

---

## 18. Find Min and Max in Array

```cpp
pair<int, int> findMinMax(vector<int>& arr) {
    int mn = arr[0], mx = arr[0];
    for (int x : arr) {
        mn = min(mn, x);
        mx = max(mx, x);
    }
    return {mn, mx};
}
// TC: O(N) | SC: O(1)
```

---

# 🟡 MEDIUM PROBLEMS

---

## 19. Two Sum Problem

**Optimal — Hashing:**

```cpp
vector<int> twoSum(vector<int>& arr, int target) {
    unordered_map<int, int> mp;
    for (int i = 0; i < arr.size(); i++) {
        int need = target - arr[i];
        if (mp.find(need) != mp.end())
            return {mp[need], i};
        mp[arr[i]] = i;
    }
    return {-1, -1};
}
// TC: O(N) | SC: O(N)
```

**Two Pointer (only if you can sort / index doesn't matter):**

```cpp
bool twoSumExists(vector<int> arr, int target) {
    sort(arr.begin(), arr.end());
    int l = 0, r = arr.size() - 1;
    while (l < r) {
        int sum = arr[l] + arr[r];
        if (sum == target) return true;
        else if (sum < target) l++;
        else r--;
    }
    return false;
}
// TC: O(N log N) | SC: O(1)
```

---

## 20. Sort 0s, 1s, 2s — Dutch National Flag

```cpp
void sortColors(vector<int>& arr) {
    int low = 0, mid = 0, high = arr.size() - 1;
    while (mid <= high) {
        if (arr[mid] == 0) swap(arr[low++], arr[mid++]);
        else if (arr[mid] == 1) mid++;
        else swap(arr[mid], arr[high--]);
    }
}
// TC: O(N) | SC: O(1) — One pass!
```

**Invariant:** `[0, low-1]` = 0s, `[low, mid-1]` = 1s, `[high+1, n-1]` = 2s.

---

## 21. Majority Element > N/2 — Boyer-Moore Voting

```cpp
int majorityElement(vector<int>& arr) {
    int count = 0, candidate = 0;
    for (int x : arr) {
        if (count == 0) candidate = x;
        count += (x == candidate) ? 1 : -1;
    }
    // Verify (skip if guaranteed)
    count = 0;
    for (int x : arr) if (x == candidate) count++;
    return (count > arr.size() / 2) ? candidate : -1;
}
// TC: O(N) | SC: O(1)
```

---

## 22. Maximum Subarray Sum — Kadane's Algorithm

```cpp
int maxSubArray(vector<int>& arr) {
    int sum = 0, maxSum = INT_MIN;
    for (int x : arr) {
        sum += x;
        maxSum = max(maxSum, sum);
        if (sum < 0) sum = 0;
    }
    return maxSum;
}
// TC: O(N) | SC: O(1)
```

> **Note:** If empty subarray is allowed, replace `INT_MIN` with `0`.

---

## 23. Print Subarray with Maximum Sum

```cpp
pair<int, pair<int,int>> maxSubArrayWithIndices(vector<int>& arr) {
    int sum = 0, maxSum = INT_MIN;
    int start = 0, ansStart = -1, ansEnd = -1;
    for (int i = 0; i < arr.size(); i++) {
        if (sum == 0) start = i;
        sum += arr[i];
        if (sum > maxSum) {
            maxSum = sum;
            ansStart = start;
            ansEnd = i;
        }
        if (sum < 0) sum = 0;
    }
    return {maxSum, {ansStart, ansEnd}};
}
// TC: O(N) | SC: O(1)
```

---

## 24. Best Time to Buy and Sell Stock (Single Transaction)

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, profit = 0;
    for (int p : prices) {
        minPrice = min(minPrice, p);
        profit = max(profit, p - minPrice);
    }
    return profit;
}
// TC: O(N) | SC: O(1)
```

---

## 25. Rearrange Array by Sign (Equal Positives & Negatives)

```cpp
vector<int> rearrangeBySign(vector<int>& arr) {
    int n = arr.size();
    vector<int> ans(n);
    int posIdx = 0, negIdx = 1;
    for (int x : arr) {
        if (x >= 0) { ans[posIdx] = x; posIdx += 2; }
        else        { ans[negIdx] = x; negIdx += 2; }
    }
    return ans;
}
// TC: O(N) | SC: O(N)
```

**Variant — Unequal counts:** place pairs first, then append leftovers.

---

## 26. Next Permutation

```cpp
void nextPermutation(vector<int>& arr) {
    int n = arr.size(), i = n - 2;
    // Step 1: Find break point (first i where arr[i] < arr[i+1])
    while (i >= 0 && arr[i] >= arr[i + 1]) i--;
    if (i >= 0) {
        // Step 2: Find smallest element > arr[i] from right
        int j = n - 1;
        while (arr[j] <= arr[i]) j--;
        swap(arr[i], arr[j]);
    }
    // Step 3: Reverse suffix
    reverse(arr.begin() + i + 1, arr.end());
}
// TC: O(N) | SC: O(1)
```

---

## 27. Leaders in an Array

**A leader is an element greater than all elements to its right.**

```cpp
vector<int> leaders(vector<int>& arr) {
    vector<int> ans;
    int n = arr.size();
    int maxRight = arr[n - 1];
    ans.push_back(maxRight);
    for (int i = n - 2; i >= 0; i--) {
        if (arr[i] > maxRight) {
            ans.push_back(arr[i]);
            maxRight = arr[i];
        }
    }
    reverse(ans.begin(), ans.end());
    return ans;
}
// TC: O(N) | SC: O(1) extra (excluding output)
```

---

## 28. Longest Consecutive Sequence

```cpp
int longestConsecutive(vector<int>& arr) {
    if (arr.empty()) return 0;
    unordered_set<int> st(arr.begin(), arr.end());
    int longest = 0;
    for (int x : st) {
        // Only start counting from sequence beginning
        if (st.find(x - 1) == st.end()) {
            int cur = x, len = 1;
            while (st.find(cur + 1) != st.end()) { cur++; len++; }
            longest = max(longest, len);
        }
    }
    return longest;
}
// TC: O(N) avg | SC: O(N)
```

---

## 29. Set Matrix Zeros

**Optimal — O(1) space using first row/col as markers:**

```cpp
void setZeroes(vector<vector<int>>& m) {
    int n = m.size(), c = m[0].size();
    bool firstRowZero = false, firstColZero = false;
    for (int j = 0; j < c; j++) if (m[0][j] == 0) firstRowZero = true;
    for (int i = 0; i < n; i++) if (m[i][0] == 0) firstColZero = true;
    for (int i = 1; i < n; i++)
        for (int j = 1; j < c; j++)
            if (m[i][j] == 0) { m[i][0] = 0; m[0][j] = 0; }
    for (int i = 1; i < n; i++)
        for (int j = 1; j < c; j++)
            if (m[i][0] == 0 || m[0][j] == 0) m[i][j] = 0;
    if (firstRowZero) for (int j = 0; j < c; j++) m[0][j] = 0;
    if (firstColZero) for (int i = 0; i < n; i++) m[i][0] = 0;
}
// TC: O(N*M) | SC: O(1)
```

---

## 30. Rotate Matrix by 90 Degrees (Clockwise)

```cpp
void rotate(vector<vector<int>>& m) {
    int n = m.size();
    // Step 1: Transpose
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            swap(m[i][j], m[j][i]);
    // Step 2: Reverse each row
    for (int i = 0; i < n; i++)
        reverse(m[i].begin(), m[i].end());
}
// TC: O(N²) | SC: O(1)
```

> **Anti-clockwise:** transpose then reverse each column (or reverse rows first, then transpose).

---

## 31. Spiral Order Matrix Traversal

```cpp
vector<int> spiralOrder(vector<vector<int>>& m) {
    vector<int> ans;
    int top = 0, bottom = m.size() - 1;
    int left = 0, right = m[0].size() - 1;
    while (top <= bottom && left <= right) {
        for (int j = left; j <= right; j++) ans.push_back(m[top][j]);
        top++;
        for (int i = top; i <= bottom; i++) ans.push_back(m[i][right]);
        right--;
        if (top <= bottom) {
            for (int j = right; j >= left; j--) ans.push_back(m[bottom][j]);
            bottom--;
        }
        if (left <= right) {
            for (int i = bottom; i >= top; i--) ans.push_back(m[i][left]);
            left++;
        }
    }
    return ans;
}
// TC: O(N*M) | SC: O(1) extra
```

---

## 32. Count Subarrays with Sum K (Positives + Negatives)

```cpp
int subarraySum(vector<int>& arr, int k) {
    unordered_map<int, int> prefixCount;
    prefixCount[0] = 1;
    int sum = 0, count = 0;
    for (int x : arr) {
        sum += x;
        if (prefixCount.find(sum - k) != prefixCount.end())
            count += prefixCount[sum - k];
        prefixCount[sum]++;
    }
    return count;
}
// TC: O(N) avg | SC: O(N)
```

---

## 33. Pascal's Triangle

**(a) Element at row r, col c:** `C(r-1, c-1)`
**(b) nth row:** Use formula `nCr = nC(r-1) * (n-r+1) / r`
**(c) Full triangle up to n rows:**

```cpp
vector<int> generateRow(int row) {
    long long ans = 1;
    vector<int> ansRow{1};
    for (int col = 1; col < row; col++) {
        ans = ans * (row - col);
        ans = ans / col;
        ansRow.push_back((int)ans);
    }
    return ansRow;
}

vector<vector<int>> pascalTriangle(int n) {
    vector<vector<int>> ans;
    for (int i = 1; i <= n; i++) ans.push_back(generateRow(i));
    return ans;
}
// TC: O(N²) | SC: O(N²) for output
```

---

## 34. Majority Element > N/3 — Extended Boyer-Moore

```cpp
vector<int> majorityElementN3(vector<int>& arr) {
    int n = arr.size();
    int c1 = 0, c2 = 0, e1 = INT_MIN, e2 = INT_MIN;
    for (int x : arr) {
        if (c1 == 0 && x != e2) { e1 = x; c1 = 1; }
        else if (c2 == 0 && x != e1) { e2 = x; c2 = 1; }
        else if (x == e1) c1++;
        else if (x == e2) c2++;
        else { c1--; c2--; }
    }
    vector<int> ans;
    int cnt1 = 0, cnt2 = 0;
    for (int x : arr) {
        if (x == e1) cnt1++;
        else if (x == e2) cnt2++;
    }
    if (cnt1 > n / 3) ans.push_back(e1);
    if (cnt2 > n / 3) ans.push_back(e2);
    return ans;
}
// TC: O(N) | SC: O(1) — At most 2 majority elements possible
```

---

## 35. 3 Sum Problem (Find All Unique Triplets That Sum to 0)

```cpp
vector<vector<int>> threeSum(vector<int>& arr) {
    sort(arr.begin(), arr.end());
    vector<vector<int>> ans;
    int n = arr.size();
    for (int i = 0; i < n; i++) {
        if (i > 0 && arr[i] == arr[i - 1]) continue;
        int j = i + 1, k = n - 1;
        while (j < k) {
            int sum = arr[i] + arr[j] + arr[k];
            if (sum < 0) j++;
            else if (sum > 0) k--;
            else {
                ans.push_back({arr[i], arr[j], arr[k]});
                j++; k--;
                while (j < k && arr[j] == arr[j - 1]) j++;
                while (j < k && arr[k] == arr[k + 1]) k--;
            }
        }
    }
    return ans;
}
// TC: O(N²) | SC: O(1) extra (excluding output)
```

---

## 36. 4 Sum Problem

```cpp
vector<vector<int>> fourSum(vector<int>& arr, int target) {
    sort(arr.begin(), arr.end());
    vector<vector<int>> ans;
    int n = arr.size();
    for (int i = 0; i < n; i++) {
        if (i > 0 && arr[i] == arr[i - 1]) continue;
        for (int j = i + 1; j < n; j++) {
            if (j > i + 1 && arr[j] == arr[j - 1]) continue;
            int k = j + 1, l = n - 1;
            while (k < l) {
                long long sum = (long long)arr[i] + arr[j] + arr[k] + arr[l];
                if (sum < target) k++;
                else if (sum > target) l--;
                else {
                    ans.push_back({arr[i], arr[j], arr[k], arr[l]});
                    k++; l--;
                    while (k < l && arr[k] == arr[k - 1]) k++;
                    while (k < l && arr[l] == arr[l + 1]) l--;
                }
            }
        }
    }
    return ans;
}
// TC: O(N³) | SC: O(1) extra
```

---

## 37. Largest Subarray with 0 Sum

```cpp
int maxLenZeroSum(vector<int>& arr) {
    unordered_map<int, int> mp;
    int sum = 0, maxLen = 0;
    for (int i = 0; i < arr.size(); i++) {
        sum += arr[i];
        if (sum == 0) maxLen = i + 1;
        else if (mp.find(sum) != mp.end())
            maxLen = max(maxLen, i - mp[sum]);
        else mp[sum] = i;
    }
    return maxLen;
}
// TC: O(N) avg | SC: O(N)
```

---

## 38. Count Subarrays with XOR K

```cpp
int subarraysXOR(vector<int>& arr, int k) {
    unordered_map<int, int> mp;
    mp[0] = 1;
    int xorSum = 0, count = 0;
    for (int x : arr) {
        xorSum ^= x;
        int need = xorSum ^ k;
        if (mp.find(need) != mp.end()) count += mp[need];
        mp[xorSum]++;
    }
    return count;
}
// TC: O(N) avg | SC: O(N)
```

---

## 39. Merge Overlapping Intervals

```cpp
vector<vector<int>> mergeIntervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> ans;
    for (auto& iv : intervals) {
        if (ans.empty() || ans.back()[1] < iv[0]) ans.push_back(iv);
        else ans.back()[1] = max(ans.back()[1], iv[1]);
    }
    return ans;
}
// TC: O(N log N) | SC: O(N) for output
```

---

## 40. Merge Two Sorted Arrays Without Extra Space

**Optimal — Gap method (Shell sort variation):**

```cpp
void mergeWithoutSpace(vector<int>& a, vector<int>& b) {
    int n = a.size(), m = b.size();
    int len = n + m;
    int gap = (len + 1) / 2;
    auto swapIfGreater = [&](int i, int j) {
        int x = (i < n) ? a[i] : b[i - n];
        int y = (j < n) ? a[j] : b[j - n];
        if (x > y) {
            if (i < n && j < n) swap(a[i], a[j]);
            else if (i < n) swap(a[i], b[j - n]);
            else swap(b[i - n], b[j - n]);
        }
    };
    while (gap > 0) {
        int left = 0, right = left + gap;
        while (right < len) {
            swapIfGreater(left, right);
            left++; right++;
        }
        if (gap == 1) break;
        gap = (gap + 1) / 2;
    }
}
// TC: O((N + M) log(N + M)) | SC: O(1)
```

**Simpler approach (when allowed):** swap elements where `a[i] > b[j]`, then sort both.

---

# 🔴 HARD PROBLEMS

---

## 41. Find Repeating and Missing Number

**Math approach (sum and sum of squares):**

```cpp
pair<int, int> findRepeatingAndMissing(vector<int>& arr) {
    long long n = arr.size();
    long long Sn  = n * (n + 1) / 2;
    long long S2n = n * (n + 1) * (2 * n + 1) / 6;
    long long S = 0, S2 = 0;
    for (int x : arr) { S += x; S2 += (long long)x * x; }
    long long val1 = S - Sn;          // x - y
    long long val2 = S2 - S2n;        // x² - y²
    long long val2_div = val2 / val1; // x + y
    long long x = (val1 + val2_div) / 2;
    long long y = x - val1;
    return {(int)x, (int)y};          // {repeating, missing}
}
// TC: O(N) | SC: O(1)
```

---

## 42. Count Inversions in Array

**Use Merge Sort — O(N log N):**

```cpp
long long mergeAndCount(vector<int>& arr, int low, int mid, int high) {
    vector<int> temp;
    int left = low, right = mid + 1;
    long long count = 0;
    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) temp.push_back(arr[left++]);
        else {
            temp.push_back(arr[right++]);
            count += (mid - left + 1);
        }
    }
    while (left <= mid)  temp.push_back(arr[left++]);
    while (right <= high) temp.push_back(arr[right++]);
    for (int i = low; i <= high; i++) arr[i] = temp[i - low];
    return count;
}

long long mergeSort(vector<int>& arr, int low, int high) {
    long long count = 0;
    if (low >= high) return count;
    int mid = (low + high) / 2;
    count += mergeSort(arr, low, mid);
    count += mergeSort(arr, mid + 1, high);
    count += mergeAndCount(arr, low, mid, high);
    return count;
}

long long countInversions(vector<int> arr) {
    return mergeSort(arr, 0, arr.size() - 1);
}
// TC: O(N log N) | SC: O(N)
```

---

## 43. Reverse Pairs (i < j and arr[i] > 2*arr[j])

```cpp
int countPairs(vector<int>& arr, int low, int mid, int high) {
    int right = mid + 1, count = 0;
    for (int i = low; i <= mid; i++) {
        while (right <= high && arr[i] > 2LL * arr[right]) right++;
        count += (right - (mid + 1));
    }
    return count;
}

void merge(vector<int>& arr, int low, int mid, int high) {
    vector<int> temp;
    int l = low, r = mid + 1;
    while (l <= mid && r <= high) {
        if (arr[l] <= arr[r]) temp.push_back(arr[l++]);
        else temp.push_back(arr[r++]);
    }
    while (l <= mid)  temp.push_back(arr[l++]);
    while (r <= high) temp.push_back(arr[r++]);
    for (int i = low; i <= high; i++) arr[i] = temp[i - low];
}

int mergeSortRP(vector<int>& arr, int low, int high) {
    if (low >= high) return 0;
    int mid = (low + high) / 2;
    int count = mergeSortRP(arr, low, mid);
    count += mergeSortRP(arr, mid + 1, high);
    count += countPairs(arr, low, mid, high);
    merge(arr, low, mid, high);
    return count;
}

int reversePairs(vector<int>& arr) {
    return mergeSortRP(arr, 0, arr.size() - 1);
}
// TC: O(N log N) | SC: O(N)
```

---

## 44. Maximum Product Subarray

```cpp
int maxProduct(vector<int>& arr) {
    int n = arr.size();
    int prefix = 1, suffix = 1, ans = INT_MIN;
    for (int i = 0; i < n; i++) {
        if (prefix == 0) prefix = 1;
        if (suffix == 0) suffix = 1;
        prefix *= arr[i];
        suffix *= arr[n - 1 - i];
        ans = max(ans, max(prefix, suffix));
    }
    return ans;
}
// TC: O(N) | SC: O(1)
```

---

## 45. Trapping Rain Water

```cpp
int trap(vector<int>& height) {
    int l = 0, r = height.size() - 1;
    int leftMax = 0, rightMax = 0, total = 0;
    while (l < r) {
        if (height[l] <= height[r]) {
            if (height[l] >= leftMax) leftMax = height[l];
            else total += leftMax - height[l];
            l++;
        } else {
            if (height[r] >= rightMax) rightMax = height[r];
            else total += rightMax - height[r];
            r--;
        }
    }
    return total;
}
// TC: O(N) | SC: O(1)
```

---

## 46. Container with Most Water

```cpp
int maxArea(vector<int>& height) {
    int l = 0, r = height.size() - 1, ans = 0;
    while (l < r) {
        int area = min(height[l], height[r]) * (r - l);
        ans = max(ans, area);
        if (height[l] < height[r]) l++;
        else r--;
    }
    return ans;
}
// TC: O(N) | SC: O(1)
```

---

## 47. Median of Two Sorted Arrays — Binary Search

```cpp
double findMedianSortedArrays(vector<int>& a, vector<int>& b) {
    if (a.size() > b.size()) return findMedianSortedArrays(b, a);
    int n1 = a.size(), n2 = b.size();
    int total = n1 + n2;
    int half = (total + 1) / 2;
    int low = 0, high = n1;
    while (low <= high) {
        int cut1 = (low + high) / 2;
        int cut2 = half - cut1;
        int l1 = (cut1 == 0)  ? INT_MIN : a[cut1 - 1];
        int l2 = (cut2 == 0)  ? INT_MIN : b[cut2 - 1];
        int r1 = (cut1 == n1) ? INT_MAX : a[cut1];
        int r2 = (cut2 == n2) ? INT_MAX : b[cut2];
        if (l1 <= r2 && l2 <= r1) {
            if (total % 2 == 1) return max(l1, l2);
            return (max(l1, l2) + min(r1, r2)) / 2.0;
        } else if (l1 > r2) high = cut1 - 1;
        else low = cut1 + 1;
    }
    return 0.0;
}
// TC: O(log(min(N, M))) | SC: O(1)
```

---

## 48. Kth Largest / Smallest Element

```cpp
// Kth Largest — min-heap of size K
int kthLargest(vector<int>& arr, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;
    for (int x : arr) {
        pq.push(x);
        if (pq.size() > k) pq.pop();
    }
    return pq.top();
}

// Kth Smallest — max-heap of size K
int kthSmallest(vector<int>& arr, int k) {
    priority_queue<int> pq;
    for (int x : arr) {
        pq.push(x);
        if (pq.size() > k) pq.pop();
    }
    return pq.top();
}
// TC: O(N log K) | SC: O(K)
```

> **Quickselect** approach achieves O(N) average time.

---

## 49. Search in Rotated Sorted Array (No Duplicates)

```cpp
int searchRotated(vector<int>& arr, int target) {
    int low = 0, high = arr.size() - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] == target) return mid;
        // Left half sorted
        if (arr[low] <= arr[mid]) {
            if (arr[low] <= target && target < arr[mid]) high = mid - 1;
            else low = mid + 1;
        } else {
            // Right half sorted
            if (arr[mid] < target && target <= arr[high]) low = mid + 1;
            else high = mid - 1;
        }
    }
    return -1;
}
// TC: O(log N) | SC: O(1)
```

**With duplicates:** when `arr[low] == arr[mid] == arr[high]`, do `low++; high--;` (worst case O(N)).

---

## 50. Find Minimum in Rotated Sorted Array

```cpp
int findMin(vector<int>& arr) {
    int low = 0, high = arr.size() - 1;
    int ans = INT_MAX;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[low] <= arr[mid]) {
            ans = min(ans, arr[low]);
            low = mid + 1;
        } else {
            ans = min(ans, arr[mid]);
            high = mid - 1;
        }
    }
    return ans;
}
// TC: O(log N) | SC: O(1)
```

---

## 51. Single Element in Sorted Array (Others Appear Twice)

```cpp
int singleNonDuplicate(vector<int>& arr) {
    int n = arr.size();
    if (n == 1) return arr[0];
    if (arr[0] != arr[1]) return arr[0];
    if (arr[n - 1] != arr[n - 2]) return arr[n - 1];
    int low = 1, high = n - 2;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] != arr[mid - 1] && arr[mid] != arr[mid + 1]) return arr[mid];
        // Left half: (even, odd) pairs → search right
        if ((mid % 2 == 0 && arr[mid] == arr[mid + 1]) ||
            (mid % 2 == 1 && arr[mid] == arr[mid - 1]))
            low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
// TC: O(log N) | SC: O(1)
```

---

## 52. Find Peak Element

```cpp
int findPeakElement(vector<int>& arr) {
    int n = arr.size();
    if (n == 1) return 0;
    if (arr[0] > arr[1]) return 0;
    if (arr[n - 1] > arr[n - 2]) return n - 1;
    int low = 1, high = n - 2;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] > arr[mid - 1] && arr[mid] > arr[mid + 1]) return mid;
        if (arr[mid] > arr[mid - 1]) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
// TC: O(log N) | SC: O(1)
```

---

## 53. Allocate Books / Capacity to Ship Packages — Binary Search on Answer

```cpp
int countStudents(vector<int>& books, int pages) {
    int students = 1, pageSum = 0;
    for (int b : books) {
        if (pageSum + b <= pages) pageSum += b;
        else { students++; pageSum = b; }
    }
    return students;
}

int allocateBooks(vector<int>& books, int m) {
    int n = books.size();
    if (m > n) return -1;
    int low = *max_element(books.begin(), books.end());
    int high = accumulate(books.begin(), books.end(), 0);
    while (low <= high) {
        int mid = (low + high) / 2;
        if (countStudents(books, mid) > m) low = mid + 1;
        else high = mid - 1;
    }
    return low;
}
// TC: O(N * log(sum - max)) | SC: O(1)
```

---

## 54. Aggressive Cows / Painter's Partition

**Aggressive Cows — place K cows in stalls with maximum minimum distance:**

```cpp
bool canPlace(vector<int>& stalls, int dist, int k) {
    int count = 1, last = stalls[0];
    for (int i = 1; i < stalls.size(); i++) {
        if (stalls[i] - last >= dist) {
            count++;
            last = stalls[i];
            if (count >= k) return true;
        }
    }
    return false;
}

int aggressiveCows(vector<int> stalls, int k) {
    sort(stalls.begin(), stalls.end());
    int low = 1, high = stalls.back() - stalls.front();
    while (low <= high) {
        int mid = (low + high) / 2;
        if (canPlace(stalls, mid, k)) low = mid + 1;
        else high = mid - 1;
    }
    return high;
}
// TC: O(N log N + N log(max - min)) | SC: O(1)
```

---

## 55. Median in Row-wise Sorted Matrix

```cpp
int countSmallEqual(vector<vector<int>>& m, int x) {
    int count = 0;
    for (auto& row : m)
        count += upper_bound(row.begin(), row.end(), x) - row.begin();
    return count;
}

int matrixMedian(vector<vector<int>>& m) {
    int n = m.size(), c = m[0].size();
    int low = INT_MAX, high = INT_MIN;
    for (int i = 0; i < n; i++) {
        low  = min(low, m[i][0]);
        high = max(high, m[i][c - 1]);
    }
    int req = (n * c) / 2;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (countSmallEqual(m, mid) <= req) low = mid + 1;
        else high = mid - 1;
    }
    return low;
}
// TC: O(log(max-min) * N * log M) | SC: O(1)
```

---

# 🎯 Bonus — Quick Reference Patterns

## ⭐ Common Patterns to Recognize

| Problem Signal | Pattern to Use |
|----------------|---------------|
| "Find pair/triplet with sum" | Two pointer (sorted) / Hashing |
| "Subarray sum / count" | Prefix sum + Hashmap |
| "Longest/shortest subarray" | Sliding window |
| "Sorted array search" | Binary search |
| "Min/max in window" | Monotonic deque |
| "Kth largest/smallest" | Heap / Quickselect |
| "All permutations/combinations" | Backtracking |
| "Maximum/minimum optimization" | DP / Kadane |
| "Range queries" | Prefix sum / Segment tree |
| "Rotation problems" | Reversal trick |
| "Duplicates in array" | Hashing / XOR / Cyclic sort |
| "Find missing number" | Sum / XOR / Cyclic sort |

---

## ⭐ Top 10 Must-Know for Tests

1. **Two Sum** — Hashing
2. **Kadane's Algorithm** — Maximum Subarray
3. **Dutch National Flag** — Sort 0,1,2
4. **Boyer-Moore** — Majority Element
5. **Reversal Algorithm** — Array Rotation
6. **Sliding Window** — Longest Subarray
7. **Prefix Sum + Hashmap** — Count Subarrays Sum K
8. **Two Pointer** — 3Sum, Trap Rain Water
9. **Binary Search on Answer** — Allocate Books
10. **Merge Sort variant** — Inversions

---

## ⭐ Pre-Test Checklist

✅ Always handle **edge cases**: empty array, single element, all duplicates, all negative
✅ Watch for **integer overflow** — use `long long` for sums of large arrays
✅ Don't forget `% n` when **rotating** by k (k can be > n)
✅ Verify candidate in **Boyer-Moore** if majority not guaranteed
✅ Check `low <= high` vs `low < high` in **binary search**
✅ Use `unordered_map` for **O(1) average** vs `map` for O(log N)
✅ For matrix problems: clarify **N×M** vs **N×N**
✅ Prefer **in-place** O(1) space when interviewer asks "can we do better?"

---

## ⭐ Useful STL Snippets

```cpp
sort(arr.begin(), arr.end());                  // ascending
sort(arr.begin(), arr.end(), greater<int>());  // descending
reverse(arr.begin(), arr.end());
int mx = *max_element(arr.begin(), arr.end());
int mn = *min_element(arr.begin(), arr.end());
int sum = accumulate(arr.begin(), arr.end(), 0);
auto it = lower_bound(arr.begin(), arr.end(), x);  // first >= x
auto it = upper_bound(arr.begin(), arr.end(), x);  // first > x
next_permutation(arr.begin(), arr.end());
prev_permutation(arr.begin(), arr.end());
```

---

# 💪 GO ACE THAT TEST!

> **Strategy on the test:**
> 1. Read the problem **twice**.
> 2. State **brute force** first — confirms understanding.
> 3. Identify the **pattern** (see table above).
> 4. Code the **optimal solution**.
> 5. **Dry run** with 1-2 test cases.
> 6. State **time & space** complexity.

**You've got this! 🚀**
