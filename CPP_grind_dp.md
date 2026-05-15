# 🧠 Complete Dynamic Programming Sheet — C++

> **The ultimate DP guide for coding interviews & tests.**
> Patterns, templates, every standard problem — memoization, tabulation, space optimization.

---

## 📑 Table of Contents

**FOUNDATIONS (1–6)**
1. What is DP? Optimal Substructure & Overlapping Subproblems
2. Memoization vs Tabulation
3. State Definition — The Heart of DP
4. 1D vs 2D vs Multi-dimensional DP
5. Space Optimization Tricks
6. How to Recognize DP Problems

**1D DP — LINEAR (7–18)**
7. Fibonacci Number
8. Climbing Stairs
9. Min Cost Climbing Stairs
10. House Robber I
11. House Robber II (Circular)
12. Maximum Subarray (Kadane's)
13. Maximum Product Subarray
14. Decode Ways
15. Jump Game I (Greedy + DP)
16. Jump Game II (Min Jumps)
17. Word Break
18. Longest Increasing Subsequence (O(N²) + O(N log N))

**2D DP — GRID (19–26)**
19. Unique Paths
20. Unique Paths II (Obstacles)
21. Minimum Path Sum
22. Triangle Minimum Path Sum
23. Dungeon Game
24. Cherry Pickup
25. Maximal Square
26. Maximal Rectangle

**KNAPSACK PATTERNS (27–34)**
27. 0/1 Knapsack
28. Subset Sum
29. Equal Subset Sum Partition
30. Target Sum (Ways)
31. Unbounded Knapsack
32. Coin Change — Min Coins
33. Coin Change II — Ways to Make Amount
34. Rod Cutting

**STRING DP (35–44)**
35. Longest Common Subsequence (LCS)
36. Longest Common Substring
37. Edit Distance (Levenshtein)
38. Distinct Subsequences
39. Wildcard Matching
40. Regular Expression Matching
41. Longest Palindromic Subsequence
42. Longest Palindromic Substring
43. Palindrome Partitioning (Min Cuts)
44. Interleaving Strings

**STOCK PROBLEMS (45–50)**
45. Best Time to Buy & Sell Stock I (One transaction)
46. Best Time to Buy & Sell Stock II (Unlimited)
47. Best Time to Buy & Sell Stock with Cooldown
48. Best Time to Buy & Sell Stock with Fee
49. Best Time to Buy & Sell Stock III (At most 2)
50. Best Time to Buy & Sell Stock IV (At most K)

**INTERVAL / MATRIX CHAIN DP (51–55)**
51. Matrix Chain Multiplication
52. Burst Balloons
53. Minimum Cost to Cut a Stick
54. Boolean Parenthesization
55. Egg Dropping Problem

**TREE / GRAPH DP (56–60)**
56. House Robber III (Tree)
57. Diameter of Tree (DP form)
58. Longest Path in DAG
59. Number of Paths in DAG
60. Cherry Pickup II (Two robots on grid)

**BITMASK DP (61–63)**
61. Travelling Salesman Problem (Bitmask)
62. Assignment Problem
63. Counting Subsets with Property

**MISCELLANEOUS HARD (64–68)**
64. Partition Equal Sum K Subsets
65. Min Number of Refueling Stops
66. Frog Jump
67. Stone Game
68. Number of Longest Increasing Subsequences

---

## ⚡ Complexity Reference

| DP Type | Typical State | Time | Space |
|---------|---------------|------|-------|
| 1D linear | `dp[i]` | O(N) | O(N) → O(1) |
| 2D grid | `dp[i][j]` | O(M × N) | O(M × N) → O(N) |
| Knapsack | `dp[i][w]` | O(N × W) | O(N × W) → O(W) |
| LCS / Edit distance | `dp[i][j]` | O(N × M) | O(N × M) → O(min(N, M)) |
| Interval | `dp[i][j]` | O(N³) | O(N²) |
| Bitmask | `dp[mask][i]` | O(2ⁿ × N) | O(2ⁿ × N) |
| Tree DP | recursion | O(N) | O(N) |

---

## 🎯 How to Recognize DP

A problem is likely DP if you see:
- **"Optimal" keywords:** minimum, maximum, longest, shortest, ways/count
- **Decision-making at each step:** "include or exclude", "take or skip"
- **Subproblems repeat:** brute force does the same work many times
- **Future depends on past:** answer for state `i` uses states `< i`

| Problem Says... | Pattern |
|-----------------|---------|
| "Count ways", "Number of paths" | Counting DP — sum of subproblems |
| "Maximum/Minimum cost/sum" | Optimization DP — min/max of subproblems |
| "Can we achieve X?" | Boolean DP — OR of subproblems |
| "Choose subset such that..." | Knapsack |
| "Compare two strings/sequences" | 2D string DP (LCS-family) |
| "Partition / Merge" | Interval DP |
| "Tree / Subtree value" | Tree DP |
| "Visit cities/subsets, small N (≤20)" | Bitmask DP |

---

# 🟢 FOUNDATIONS

---

## 1. What is DP?

**Two ingredients required:**

1. **Optimal Substructure** — Optimal solution to the problem contains optimal solutions to subproblems.
   - Shortest path A→C through B = shortest A→B + shortest B→C
2. **Overlapping Subproblems** — Same subproblems solved many times.
   - `fib(5)` computes `fib(3)` twice, `fib(2)` three times, etc.

**Without #2, recursion alone is fine — no DP needed (e.g., merge sort).**
**Without #1, DP doesn't apply (e.g., longest simple path in arbitrary graph — NP-hard).**

```
Brute force fib(5):           DP fib(5):
fib(5)                        Memo: { }
├─ fib(4)                     fib(5) → fib(4), fib(3)
│  ├─ fib(3)                  fib(4) → fib(3), fib(2)
│  │  ├─ fib(2)               fib(3) → fib(2), fib(1)   stored: 2
│  │  │  ├─ fib(1)            fib(2) → fib(1), fib(0)   stored: 1
│  │  │  └─ fib(0)            ...
│  │  └─ fib(1)               Re-use stored values.
│  └─ fib(2)                  O(N) instead of O(2^N).
│     └─ ...
└─ fib(3)
   └─ ...
```

---

## 2. Memoization vs Tabulation

**Two ways to do DP:**

### Memoization (Top-Down — Recursion + Cache)

```cpp
unordered_map<int,int> memo;
int fib(int n) {
    if (n <= 1) return n;
    if (memo.count(n)) return memo[n];
    return memo[n] = fib(n - 1) + fib(n - 2);
}
```

✅ Natural to write (just add memo to recursion)
✅ Only computes states you actually need
❌ Recursion stack overhead
❌ Risk of stack overflow for deep recursion

### Tabulation (Bottom-Up — Iterative Table)

```cpp
int fib(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) dp[i] = dp[i - 1] + dp[i - 2];
    return dp[n];
}
```

✅ No recursion overhead — faster constants
✅ Easy to space-optimize
✅ Iterative — no stack overflow
❌ Must figure out the right order to fill the table
❌ Sometimes fills unneeded states

> **Test strategy:** Write memoization first (easy), convert to tabulation if needed for performance.

---

## 3. State Definition — The Heart of DP

**Most important step.** Define what `dp[...]` MEANS.

Examples:
- `dp[i]` = LIS ending exactly at index i
- `dp[i][j]` = LCS of `s1[0..i-1]` and `s2[0..j-1]`
- `dp[i][w]` = max value using first i items, capacity w
- `dp[mask][i]` = min cost to visit cities in mask, currently at i

**Bad state definition → unsolvable problem.**

A good state must:
- **Capture enough information** to make decisions for the next step
- **Be polynomially small** (else DP isn't tractable)
- **Have clear base cases**

---

## 4. 1D vs 2D vs Multi-D DP

| Dim | Example State | Examples |
|-----|---------------|----------|
| **1D** | `dp[i]` | Fibonacci, LIS, House Robber, Climbing Stairs |
| **2D** | `dp[i][j]` | LCS, Edit Distance, Grid paths, Knapsack |
| **3D** | `dp[i][j][k]` | Cherry Pickup II, K-transactions stock, palindrome partitioning |
| **Bitmask** | `dp[mask][i]` | TSP, assignment problems |
| **Tree DP** | `dp[node][state]` | Tree problems via DFS returning multiple values |

---

## 5. Space Optimization Tricks

If `dp[i]` only depends on `dp[i-1]` (or last K rows) — keep only those.

```cpp
// Fibonacci with O(1) space
int fib(int n) {
    if (n <= 1) return n;
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr; curr = next;
    }
    return curr;
}

// 2D → 1D when only previous row needed
// Iterate carefully — forward vs reverse depends on dependency direction.
```

> **Rule of thumb:** Optimize only when N is huge. Readability > 5% memory savings.

---

## 6. How to Recognize DP — A Checklist

✅ Can you describe the problem with a recursive formula?
✅ Do subproblems repeat?
✅ Does the answer depend on choices made at earlier steps?
✅ Asked for **min / max / count / can-achieve**?

If yes to most → likely DP.

**General template:**

```
1. Define state: dp[...] = "..."
2. Write recurrence: dp[...] = f(dp[...], ...)
3. Base cases
4. Order of evaluation (for tabulation)
5. Space optimization (if needed)
6. Return: which dp[...] is the answer?
```

---

# 🟢 1D DP — LINEAR

---

## 7. Fibonacci

```cpp
// Memo
int fibMemo(int n, vector<int>& memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    return memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
}

// Tabulation
int fibTab(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) dp[i] = dp[i-1] + dp[i-2];
    return dp[n];
}

// Space-optimized
int fib(int n) {
    if (n <= 1) return n;
    int a = 0, b = 1;
    for (int i = 2; i <= n; i++) { int c = a + b; a = b; b = c; }
    return b;
}
// TC: O(N) | SC: O(1)
```

---

## 8. Climbing Stairs

**Each step: take 1 or 2 stairs. How many ways to reach top?**

```cpp
int climbStairs(int n) {
    if (n <= 2) return n;
    int a = 1, b = 2;
    for (int i = 3; i <= n; i++) { int c = a + b; a = b; b = c; }
    return b;
}
// TC: O(N) | SC: O(1)
// State: dp[i] = ways to reach step i
// Recurrence: dp[i] = dp[i-1] + dp[i-2]  (came from step i-1 or i-2)
```

---

## 9. Min Cost Climbing Stairs

```cpp
int minCostClimbingStairs(vector<int>& cost) {
    int n = cost.size();
    int a = 0, b = 0;
    for (int i = 2; i <= n; i++) {
        int c = min(a + cost[i-2], b + cost[i-1]);
        a = b; b = c;
    }
    return b;
}
// TC: O(N) | SC: O(1)
// dp[i] = min cost to reach step i
// dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
```

---

## 10. House Robber I

**Can't rob adjacent houses. Max amount?**

```cpp
int rob(vector<int>& nums) {
    int prev2 = 0, prev1 = 0;
    for (int n : nums) {
        int cur = max(prev1, prev2 + n);
        prev2 = prev1;
        prev1 = cur;
    }
    return prev1;
}
// TC: O(N) | SC: O(1)
// State: dp[i] = max money from houses[0..i]
// Choice: rob house i (dp[i-2] + nums[i]) or skip (dp[i-1])
```

---

## 11. House Robber II (Circular)

**First and last are adjacent. Trick: rob `[0..n-2]` OR `[1..n-1]`, take max.**

```cpp
int robRange(vector<int>& nums, int l, int r) {
    int prev2 = 0, prev1 = 0;
    for (int i = l; i <= r; i++) {
        int cur = max(prev1, prev2 + nums[i]);
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int rob(vector<int>& nums) {
    if (nums.size() == 1) return nums[0];
    return max(robRange(nums, 0, nums.size() - 2),
               robRange(nums, 1, nums.size() - 1));
}
// TC: O(N) | SC: O(1)
```

---

## 12. Maximum Subarray — Kadane's Algorithm

```cpp
int maxSubArray(vector<int>& nums) {
    int curSum = nums[0], maxSum = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        curSum = max(nums[i], curSum + nums[i]);   // extend or restart
        maxSum = max(maxSum, curSum);
    }
    return maxSum;
}
// TC: O(N) | SC: O(1)
// State: dp[i] = max subarray sum ending exactly at i
// dp[i] = max(nums[i], dp[i-1] + nums[i])
```

---

## 13. Maximum Product Subarray

**Trick: track both min AND max — negatives flip signs.**

```cpp
int maxProduct(vector<int>& nums) {
    int curMin = nums[0], curMax = nums[0], ans = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        if (nums[i] < 0) swap(curMin, curMax);
        curMax = max(nums[i], curMax * nums[i]);
        curMin = min(nums[i], curMin * nums[i]);
        ans = max(ans, curMax);
    }
    return ans;
}
// TC: O(N) | SC: O(1)
```

---

## 14. Decode Ways

**String of digits → letters (A=1...Z=26). How many decodings?**

```cpp
int numDecodings(string s) {
    int n = s.size();
    if (s[0] == '0') return 0;
    int prev2 = 1, prev1 = 1;
    for (int i = 1; i < n; i++) {
        int cur = 0;
        if (s[i] != '0') cur += prev1;                       // single digit
        int two = (s[i-1] - '0') * 10 + (s[i] - '0');
        if (two >= 10 && two <= 26) cur += prev2;            // two-digit
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}
// TC: O(N) | SC: O(1)
// dp[i] = ways to decode s[0..i]
```

---

## 15. Jump Game I

**Can you reach the last index?** (Pure greedy is easier than DP here.)

```cpp
bool canJump(vector<int>& nums) {
    int maxReach = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (i > maxReach) return false;
        maxReach = max(maxReach, i + nums[i]);
    }
    return true;
}
// TC: O(N) | SC: O(1)

// DP version
bool canJumpDP(vector<int>& nums) {
    int n = nums.size();
    vector<bool> dp(n, false);
    dp[0] = true;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && j + nums[j] >= i) { dp[i] = true; break; }
        }
    }
    return dp[n-1];
}
// TC: O(N²) | SC: O(N)
```

---

## 16. Jump Game II — Min Jumps

```cpp
int jump(vector<int>& nums) {
    int n = nums.size(), jumps = 0, curEnd = 0, farthest = 0;
    for (int i = 0; i < n - 1; i++) {
        farthest = max(farthest, i + nums[i]);
        if (i == curEnd) {                       // finished current jump's range
            jumps++;
            curEnd = farthest;
        }
    }
    return jumps;
}
// TC: O(N) | SC: O(1)  — BFS-like greedy
```

---

## 17. Word Break

**Can `s` be segmented into words from dict?**

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    int n = s.size();
    vector<bool> dp(n + 1, false);
    dp[0] = true;
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && dict.count(s.substr(j, i - j))) { dp[i] = true; break; }
        }
    }
    return dp[n];
}
// TC: O(N² × L) | SC: O(N)
// dp[i] = can s[0..i-1] be segmented?
```

---

## 18. Longest Increasing Subsequence (LIS)

```cpp
// O(N²) DP
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size(), best = 1;
    vector<int> dp(n, 1);                        // dp[i] = LIS ending at i
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) dp[i] = max(dp[i], dp[j] + 1);
        }
        best = max(best, dp[i]);
    }
    return best;
}

// O(N log N) — patience sorting
int lengthOfLISFast(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}
// tails[i] = smallest tail value of any LIS of length i+1
```

> **Key insight:** the O(N log N) version's `tails` is NOT the LIS itself — just its length is correct.

---

# 🟢 2D DP — GRID

---

## 19. Unique Paths

**M × N grid, move only right/down. How many paths top-left → bottom-right?**

```cpp
int uniquePaths(int m, int n) {
    vector<vector<int>> dp(m, vector<int>(n, 1));
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
    return dp[m-1][n-1];
}
// TC: O(M × N) | SC: O(M × N) → O(N) with 1D dp

// O(N) space
int uniquePathsOptimized(int m, int n) {
    vector<int> dp(n, 1);
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[j] += dp[j-1];
    return dp[n-1];
}
```

---

## 20. Unique Paths II (Obstacles)

```cpp
int uniquePathsWithObstacles(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    if (grid[0][0] == 1) return 0;
    vector<vector<long>> dp(m, vector<long>(n, 0));
    dp[0][0] = 1;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) { dp[i][j] = 0; continue; }
            if (i > 0) dp[i][j] += dp[i-1][j];
            if (j > 0) dp[i][j] += dp[i][j-1];
        }
    }
    return dp[m-1][n-1];
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 21. Minimum Path Sum

```cpp
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) continue;
            int top  = (i > 0) ? grid[i-1][j] : INT_MAX;
            int left = (j > 0) ? grid[i][j-1] : INT_MAX;
            grid[i][j] += min(top, left);
        }
    }
    return grid[m-1][n-1];
}
// TC: O(M × N) | SC: O(1) — modifying grid in-place
```

---

## 22. Triangle Minimum Path Sum

```cpp
int minimumTotal(vector<vector<int>>& tri) {
    int n = tri.size();
    vector<int> dp = tri.back();
    for (int i = n - 2; i >= 0; i--) {
        for (int j = 0; j <= i; j++) {
            dp[j] = tri[i][j] + min(dp[j], dp[j+1]);
        }
    }
    return dp[0];
}
// TC: O(N²) | SC: O(N)  — bottom-up from last row
```

---

## 23. Dungeon Game (Hard)

**Knight enters top-left, exits bottom-right. Each cell adds/subtracts HP. Min initial HP needed?**

```cpp
int calculateMinimumHP(vector<vector<int>>& dungeon) {
    int m = dungeon.size(), n = dungeon[0].size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, INT_MAX));
    dp[m][n-1] = dp[m-1][n] = 1;
    for (int i = m - 1; i >= 0; i--) {
        for (int j = n - 1; j >= 0; j--) {
            int need = min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j];
            dp[i][j] = max(1, need);                       // HP can't drop ≤ 0
        }
    }
    return dp[0][0];
}
// TC: O(M × N) | SC: O(M × N)
// IMPORTANT: solve from bottom-right back to top-left.
```

---

## 24. Cherry Pickup

**Round-trip top-left → bottom-right → top-left, collect max cherries.** Trick: 2 paths going simultaneously, 3D state.

```cpp
int cherryPickup(vector<vector<int>>& grid) {
    int n = grid.size();
    vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(n, INT_MIN)));
    dp[0][0][0] = grid[0][0];
    for (int x1 = 0; x1 < n; x1++) {
        for (int y1 = 0; y1 < n; y1++) {
            for (int x2 = 0; x2 < n; x2++) {
                int y2 = x1 + y1 - x2;
                if (y2 < 0 || y2 >= n) continue;
                if (grid[x1][y1] < 0 || grid[x2][y2] < 0) continue;
                int cherries = grid[x1][y1] + (x1 != x2 ? grid[x2][y2] : 0);
                int best = INT_MIN;
                if (x1 && x2 && dp[x1-1][y1][x2-1] != INT_MIN) best = max(best, dp[x1-1][y1][x2-1]);
                if (x1 && y2 && dp[x1-1][y1][x2]   != INT_MIN) best = max(best, dp[x1-1][y1][x2]);
                if (y1 && x2 && dp[x1][y1-1][x2-1] != INT_MIN) best = max(best, dp[x1][y1-1][x2-1]);
                if (y1 && y2 && dp[x1][y1-1][x2]   != INT_MIN) best = max(best, dp[x1][y1-1][x2]);
                if (x1 == 0 && y1 == 0 && x2 == 0) best = 0;
                if (best != INT_MIN) dp[x1][y1][x2] = best + cherries;
            }
        }
    }
    return max(0, dp[n-1][n-1][n-1]);
}
// TC: O(N³) | SC: O(N³)
```

---

## 25. Maximal Square

**Largest square of 1's in binary matrix.**

```cpp
int maximalSquare(vector<vector<char>>& matrix) {
    int m = matrix.size(), n = matrix[0].size(), best = 0;
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (matrix[i-1][j-1] == '1') {
                dp[i][j] = min({dp[i-1][j-1], dp[i-1][j], dp[i][j-1]}) + 1;
                best = max(best, dp[i][j]);
            }
        }
    }
    return best * best;
}
// TC: O(M × N) | SC: O(M × N) → O(N)
// dp[i][j] = side of largest square ENDING at (i,j)
```

---

## 26. Maximal Rectangle

**Largest rectangle of 1's in binary matrix.** Reduce to "Largest Rectangle in Histogram" per row.

```cpp
int largestRectangleArea(vector<int>& h) {
    int n = h.size(), best = 0;
    stack<int> st;
    for (int i = 0; i <= n; i++) {
        int cur = (i == n) ? 0 : h[i];
        while (!st.empty() && h[st.top()] > cur) {
            int height = h[st.top()]; st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            best = max(best, height * width);
        }
        st.push(i);
    }
    return best;
}

int maximalRectangle(vector<vector<char>>& matrix) {
    if (matrix.empty()) return 0;
    int m = matrix.size(), n = matrix[0].size(), best = 0;
    vector<int> heights(n, 0);
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            heights[j] = matrix[i][j] == '1' ? heights[j] + 1 : 0;
        }
        best = max(best, largestRectangleArea(heights));
    }
    return best;
}
// TC: O(M × N) | SC: O(N)
```

---

# 💼 KNAPSACK PATTERNS

---

## 27. 0/1 Knapsack

**N items (weight + value), capacity W. Max value, each item 0/1 times.**

```cpp
int knapsack01(vector<int>& wt, vector<int>& val, int W) {
    int n = wt.size();
    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            dp[i][w] = dp[i-1][w];                          // skip item i-1
            if (wt[i-1] <= w)
                dp[i][w] = max(dp[i][w], dp[i-1][w - wt[i-1]] + val[i-1]);
        }
    }
    return dp[n][W];
}
// TC: O(N × W) | SC: O(N × W) → O(W)

// 1D space-optimized — iterate w in REVERSE
int knapsack01Optim(vector<int>& wt, vector<int>& val, int W) {
    int n = wt.size();
    vector<int> dp(W + 1, 0);
    for (int i = 0; i < n; i++) {
        for (int w = W; w >= wt[i]; w--) {                 // reverse!
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
        }
    }
    return dp[W];
}
```

> **Why reverse?** In 1D, going forward would re-use item i multiple times (= unbounded knapsack). Reverse keeps it 0/1.

---

## 28. Subset Sum

**Can we pick a subset summing to T?**

```cpp
bool subsetSum(vector<int>& nums, int T) {
    int n = nums.size();
    vector<bool> dp(T + 1, false);
    dp[0] = true;
    for (int x : nums) {
        for (int t = T; t >= x; t--) {
            dp[t] = dp[t] || dp[t - x];
        }
    }
    return dp[T];
}
// TC: O(N × T) | SC: O(T)
```

---

## 29. Equal Subset Sum Partition

**Split array into 2 subsets with equal sum.** Reduce to subset sum = total/2.

```cpp
bool canPartition(vector<int>& nums) {
    int sum = accumulate(nums.begin(), nums.end(), 0);
    if (sum % 2) return false;
    return subsetSum(nums, sum / 2);
}
// TC: O(N × sum) | SC: O(sum)
```

---

## 30. Target Sum — Number of Ways

**Assign +/- to each number to reach target.** Trick: convert to subset sum count.

```cpp
// total = sum(nums); pos = subset with +, neg = with -;
// pos + neg = total, pos - neg = target
// pos = (total + target) / 2
int findTargetSumWays(vector<int>& nums, int target) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if ((total + target) % 2 != 0 || abs(target) > total) return 0;
    int t = (total + target) / 2;
    vector<int> dp(t + 1, 0);
    dp[0] = 1;
    for (int x : nums) {
        for (int s = t; s >= x; s--) dp[s] += dp[s - x];
    }
    return dp[t];
}
// TC: O(N × t) | SC: O(t)
```

---

## 31. Unbounded Knapsack

**Each item can be used UNLIMITED times.** Iterate w FORWARD.

```cpp
int unboundedKnapsack(vector<int>& wt, vector<int>& val, int W) {
    int n = wt.size();
    vector<int> dp(W + 1, 0);
    for (int i = 0; i < n; i++) {
        for (int w = wt[i]; w <= W; w++) {                 // forward
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
        }
    }
    return dp[W];
}
// TC: O(N × W) | SC: O(W)
```

---

## 32. Coin Change — Min Coins

```cpp
int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, INT_MAX);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
        for (int c : coins) {
            if (c <= i && dp[i - c] != INT_MAX)
                dp[i] = min(dp[i], dp[i - c] + 1);
        }
    }
    return dp[amount] == INT_MAX ? -1 : dp[amount];
}
// TC: O(amount × coins) | SC: O(amount)
```

---

## 33. Coin Change II — Number of Ways

```cpp
int change(int amount, vector<int>& coins) {
    vector<int> dp(amount + 1, 0);
    dp[0] = 1;
    for (int c : coins) {                                  // coin outer for COMBINATIONS
        for (int a = c; a <= amount; a++) {
            dp[a] += dp[a - c];
        }
    }
    return dp[amount];
}
// TC: O(amount × coins) | SC: O(amount)
```

> **Loop order matters!**
> - **Coin outer, amount inner** → combinations (order doesn't matter)
> - **Amount outer, coin inner** → permutations (order matters)

---

## 34. Rod Cutting

**Rod of length N. Prices for lengths 1..N. Max revenue?**

```cpp
int rodCutting(vector<int>& price, int n) {
    vector<int> dp(n + 1, 0);
    for (int len = 1; len <= n; len++) {
        for (int cut = 1; cut <= len; cut++) {
            dp[len] = max(dp[len], price[cut - 1] + dp[len - cut]);
        }
    }
    return dp[n];
}
// TC: O(N²) | SC: O(N) — unbounded knapsack pattern
```

---

# 🔤 STRING DP

---

## 35. Longest Common Subsequence (LCS)

```cpp
int lcs(string& a, string& b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (a[i-1] == b[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
            else                  dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[m][n];
}
// TC: O(M × N) | SC: O(M × N) → O(min(M, N))

// Reconstruct the LCS
string reconstructLCS(string& a, string& b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = (a[i-1] == b[j-1]) ? dp[i-1][j-1] + 1
                                          : max(dp[i-1][j], dp[i][j-1]);
    string result;
    int i = m, j = n;
    while (i > 0 && j > 0) {
        if (a[i-1] == b[j-1]) { result += a[i-1]; i--; j--; }
        else if (dp[i-1][j] > dp[i][j-1]) i--;
        else j--;
    }
    reverse(result.begin(), result.end());
    return result;
}
```

---

## 36. Longest Common Substring

```cpp
int longestCommonSubstring(string& a, string& b) {
    int m = a.size(), n = b.size(), best = 0;
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (a[i-1] == b[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
                best = max(best, dp[i][j]);
            }
            // else: dp[i][j] = 0 (substring must be contiguous!)
        }
    }
    return best;
}
// TC: O(M × N) | SC: O(M × N)
```

> **Difference from LCS:** substring must be contiguous → reset to 0 on mismatch.

---

## 37. Edit Distance (Levenshtein)

**Min operations (insert/delete/replace) to convert a → b.**

```cpp
int editDistance(string a, string b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1));
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (a[i-1] == b[j-1]) dp[i][j] = dp[i-1][j-1];
            else dp[i][j] = 1 + min({dp[i-1][j],   // delete
                                     dp[i][j-1],   // insert
                                     dp[i-1][j-1]});  // replace
        }
    }
    return dp[m][n];
}
// TC: O(M × N) | SC: O(M × N) → O(N)
```

---

## 38. Distinct Subsequences

**Number of times `t` appears as a subsequence in `s`.**

```cpp
int numDistinct(string s, string t) {
    int m = s.size(), n = t.size();
    vector<vector<long>> dp(m + 1, vector<long>(n + 1, 0));
    for (int i = 0; i <= m; i++) dp[i][0] = 1;
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            dp[i][j] = dp[i-1][j];                  // skip s[i-1]
            if (s[i-1] == t[j-1]) dp[i][j] += dp[i-1][j-1];   // also use it
        }
    }
    return (int)dp[m][n];
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 39. Wildcard Matching (`*` matches any sequence, `?` matches one char)

```cpp
bool isMatch(string s, string p) {
    int m = s.size(), n = p.size();
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    for (int j = 1; j <= n; j++)
        if (p[j-1] == '*') dp[0][j] = dp[0][j-1];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p[j-1] == '*')      dp[i][j] = dp[i-1][j] || dp[i][j-1];
            else if (p[j-1] == '?' || s[i-1] == p[j-1])
                                    dp[i][j] = dp[i-1][j-1];
        }
    }
    return dp[m][n];
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 40. Regular Expression Matching (`*` = 0+ of previous, `.` = any char)

```cpp
bool isMatchRegex(string s, string p) {
    int m = s.size(), n = p.size();
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    for (int j = 2; j <= n; j++)
        if (p[j-1] == '*') dp[0][j] = dp[0][j-2];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p[j-1] == '*') {
                dp[i][j] = dp[i][j-2];                              // 0 occurrences
                if (p[j-2] == '.' || p[j-2] == s[i-1])
                    dp[i][j] = dp[i][j] || dp[i-1][j];              // 1+ occurrences
            } else if (p[j-1] == '.' || p[j-1] == s[i-1]) {
                dp[i][j] = dp[i-1][j-1];
            }
        }
    }
    return dp[m][n];
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 41. Longest Palindromic Subsequence

**LCS of `s` and reverse(`s`).**

```cpp
int longestPalindromicSubseq(string s) {
    string r = s;
    reverse(r.begin(), r.end());
    return lcs(s, r);
}
// TC: O(N²) | SC: O(N²)
```

---

## 42. Longest Palindromic Substring

**Use 2D DP or expand-around-center (simpler).**

```cpp
// Expand around center — O(N²) time, O(1) space
string longestPalindrome(string s) {
    int n = s.size(), start = 0, maxLen = 1;
    auto expand = [&](int l, int r) {
        while (l >= 0 && r < n && s[l] == s[r]) { l--; r++; }
        if (r - l - 1 > maxLen) { maxLen = r - l - 1; start = l + 1; }
    };
    for (int i = 0; i < n; i++) {
        expand(i, i);          // odd length
        expand(i, i + 1);      // even length
    }
    return s.substr(start, maxLen);
}
// TC: O(N²) | SC: O(1)

// DP version (also useful base for palindrome partitioning)
string longestPalindromeDP(string s) {
    int n = s.size(), start = 0, maxLen = 1;
    vector<vector<bool>> dp(n, vector<bool>(n, false));
    for (int i = 0; i < n; i++) dp[i][i] = true;
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i + len <= n; i++) {
            int j = i + len - 1;
            if (s[i] == s[j] && (len == 2 || dp[i+1][j-1])) {
                dp[i][j] = true;
                if (len > maxLen) { maxLen = len; start = i; }
            }
        }
    }
    return s.substr(start, maxLen);
}
```

---

## 43. Palindrome Partitioning — Min Cuts

```cpp
int minCut(string s) {
    int n = s.size();
    vector<vector<bool>> pal(n, vector<bool>(n, false));
    for (int i = n - 1; i >= 0; i--) {
        for (int j = i; j < n; j++) {
            if (s[i] == s[j] && (j - i <= 1 || pal[i+1][j-1])) pal[i][j] = true;
        }
    }
    vector<int> dp(n, 0);
    for (int i = 0; i < n; i++) {
        if (pal[0][i]) { dp[i] = 0; continue; }
        dp[i] = INT_MAX;
        for (int j = 1; j <= i; j++) {
            if (pal[j][i]) dp[i] = min(dp[i], dp[j-1] + 1);
        }
    }
    return dp[n-1];
}
// TC: O(N²) | SC: O(N²)
```

---

## 44. Interleaving Strings

**Is `s3` an interleaving of `s1` and `s2`?** `dp[i][j]` = can first `i+j` chars of s3 be formed from first i of s1 + first j of s2.

```cpp
bool isInterleave(string s1, string s2, string s3) {
    int m = s1.size(), n = s2.size();
    if (m + n != s3.size()) return false;
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    for (int i = 0; i <= m; i++) {
        for (int j = 0; j <= n; j++) {
            if (i > 0) dp[i][j] = dp[i][j] || (dp[i-1][j] && s1[i-1] == s3[i+j-1]);
            if (j > 0) dp[i][j] = dp[i][j] || (dp[i][j-1] && s2[j-1] == s3[i+j-1]);
        }
    }
    return dp[m][n];
}
// TC: O(M × N) | SC: O(M × N)
```

---

# 📈 STOCK PROBLEMS — STATE MACHINE PATTERN

---

## 45. Best Time I — At Most One Transaction

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, best = 0;
    for (int p : prices) {
        minPrice = min(minPrice, p);
        best = max(best, p - minPrice);
    }
    return best;
}
// TC: O(N) | SC: O(1)
```

---

## 46. Best Time II — Unlimited Transactions

```cpp
int maxProfitII(vector<int>& prices) {
    int profit = 0;
    for (int i = 1; i < prices.size(); i++)
        if (prices[i] > prices[i-1]) profit += prices[i] - prices[i-1];
    return profit;
}
// TC: O(N) | SC: O(1) — capture every uphill
```

---

## 47. Stock with Cooldown

**After selling, must skip a day.**

```cpp
int maxProfitCooldown(vector<int>& prices) {
    int n = prices.size();
    if (n == 0) return 0;
    int hold = -prices[0], sold = 0, rest = 0;
    for (int i = 1; i < n; i++) {
        int prevSold = sold;
        sold = hold + prices[i];
        hold = max(hold, rest - prices[i]);
        rest = max(rest, prevSold);
    }
    return max(sold, rest);
}
// TC: O(N) | SC: O(1)
// 3 states: hold (own stock), sold (just sold today), rest (idle)
```

---

## 48. Stock with Transaction Fee

```cpp
int maxProfitFee(vector<int>& prices, int fee) {
    int hold = -prices[0], cash = 0;
    for (int i = 1; i < prices.size(); i++) {
        int prevCash = cash;
        cash = max(cash, hold + prices[i] - fee);
        hold = max(hold, prevCash - prices[i]);
    }
    return cash;
}
// TC: O(N) | SC: O(1)
```

---

## 49. Best Time III — At Most 2 Transactions

```cpp
int maxProfitIII(vector<int>& prices) {
    int buy1 = INT_MIN, sell1 = 0, buy2 = INT_MIN, sell2 = 0;
    for (int p : prices) {
        buy1  = max(buy1, -p);
        sell1 = max(sell1, buy1 + p);
        buy2  = max(buy2, sell1 - p);
        sell2 = max(sell2, buy2 + p);
    }
    return sell2;
}
// TC: O(N) | SC: O(1)
```

---

## 50. Best Time IV — At Most K Transactions

```cpp
int maxProfitK(int k, vector<int>& prices) {
    int n = prices.size();
    if (n == 0 || k == 0) return 0;
    if (k >= n / 2) {                                       // basically unlimited
        int profit = 0;
        for (int i = 1; i < n; i++)
            if (prices[i] > prices[i-1]) profit += prices[i] - prices[i-1];
        return profit;
    }
    vector<int> buy(k + 1, INT_MIN), sell(k + 1, 0);
    for (int p : prices) {
        for (int j = 1; j <= k; j++) {
            buy[j]  = max(buy[j],  sell[j-1] - p);
            sell[j] = max(sell[j], buy[j]    + p);
        }
    }
    return sell[k];
}
// TC: O(N × K) | SC: O(K)
```

---

# 🧱 INTERVAL / MATRIX CHAIN DP

---

## 51. Matrix Chain Multiplication

**Min scalar multiplications for chain.**

```cpp
int matrixChain(vector<int>& dims) {
    int n = dims.size() - 1;                                // n matrices
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i + len <= n; i++) {
            int j = i + len - 1;
            dp[i][j] = INT_MAX;
            for (int k = i; k < j; k++) {
                int cost = dp[i][k] + dp[k+1][j] + dims[i] * dims[k+1] * dims[j+1];
                dp[i][j] = min(dp[i][j], cost);
            }
        }
    }
    return dp[0][n-1];
}
// TC: O(N³) | SC: O(N²)
```

---

## 52. Burst Balloons (LeetCode 312)

**Burst balloons, gain `nums[l] × nums[i] × nums[r]`. Max total.** Trick: think LAST balloon burst.

```cpp
int maxCoins(vector<int>& nums) {
    int n = nums.size();
    vector<int> a(n + 2, 1);
    for (int i = 0; i < n; i++) a[i + 1] = nums[i];
    vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));
    for (int len = 1; len <= n; len++) {
        for (int l = 1; l + len <= n + 1; l++) {
            int r = l + len - 1;
            for (int k = l; k <= r; k++) {                  // k = last burst
                int gain = a[l-1] * a[k] * a[r+1] + dp[l][k-1] + dp[k+1][r];
                dp[l][r] = max(dp[l][r], gain);
            }
        }
    }
    return dp[1][n];
}
// TC: O(N³) | SC: O(N²)
```

---

## 53. Minimum Cost to Cut a Stick (LeetCode 1547)

```cpp
int minCost(int n, vector<int>& cuts) {
    cuts.push_back(0); cuts.push_back(n);
    sort(cuts.begin(), cuts.end());
    int m = cuts.size();
    vector<vector<int>> dp(m, vector<int>(m, 0));
    for (int len = 2; len < m; len++) {
        for (int l = 0; l + len < m; l++) {
            int r = l + len;
            dp[l][r] = INT_MAX;
            for (int k = l + 1; k < r; k++) {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k][r] + cuts[r] - cuts[l]);
            }
        }
    }
    return dp[0][m-1];
}
// TC: O(M³) | SC: O(M²)
```

---

## 54. Boolean Parenthesization

**Number of ways to parenthesize boolean expression to evaluate True.**

```cpp
int countWays(string s) {
    int n = s.size();
    vector<vector<int>> T(n, vector<int>(n, 0)), F(n, vector<int>(n, 0));
    for (int i = 0; i < n; i += 2) {
        T[i][i] = (s[i] == 'T');
        F[i][i] = (s[i] == 'F');
    }
    for (int len = 3; len <= n; len += 2) {
        for (int i = 0; i + len <= n; i += 2) {
            int j = i + len - 1;
            for (int k = i + 1; k < j; k += 2) {
                int lt = T[i][k-1], lf = F[i][k-1];
                int rt = T[k+1][j], rf = F[k+1][j];
                char op = s[k];
                if (op == '&') { T[i][j] += lt*rt; F[i][j] += lt*rf + lf*rt + lf*rf; }
                else if (op == '|') { T[i][j] += lt*rt + lt*rf + lf*rt; F[i][j] += lf*rf; }
                else { T[i][j] += lt*rf + lf*rt; F[i][j] += lt*rt + lf*rf; }
            }
        }
    }
    return T[0][n-1];
}
// TC: O(N³) | SC: O(N²)
```

---

## 55. Egg Dropping Problem

**K eggs, N floors. Min moves to find critical floor in worst case.**

```cpp
int superEggDrop(int K, int N) {
    vector<vector<int>> dp(K + 1, vector<int>(N + 1, 0));
    int m = 0;
    while (dp[K][m] < N) {
        m++;
        for (int k = 1; k <= K; k++)
            dp[k][m] = dp[k-1][m-1] + dp[k][m-1] + 1;
    }
    return m;
}
// TC: O(K × log N) | SC: O(K × log N)
// Genius trick: dp[k][m] = max floors testable with k eggs and m moves
```

---

# 🌲 TREE & GRAPH DP

---

## 56. House Robber III (Tree)

**Adjacent nodes can't both be robbed.**

```cpp
struct TreeNode { int val; TreeNode *left, *right; };

pair<int,int> robHelper(TreeNode* root) {        // {with_root, without_root}
    if (!root) return {0, 0};
    auto [lw, lwo] = robHelper(root->left);
    auto [rw, rwo] = robHelper(root->right);
    int with = root->val + lwo + rwo;            // rob root → can't rob children
    int without = max(lw, lwo) + max(rw, rwo);   // skip root → free to choose
    return {with, without};
}

int robTree(TreeNode* root) {
    auto [w, wo] = robHelper(root);
    return max(w, wo);
}
// TC: O(N) | SC: O(H)
```

---

## 57. Diameter of Tree (DP form)

```cpp
int diameter = 0;
int dfsDiameter(TreeNode* root) {
    if (!root) return 0;
    int l = dfsDiameter(root->left);
    int r = dfsDiameter(root->right);
    diameter = max(diameter, l + r);
    return 1 + max(l, r);
}
// TC: O(N) | SC: O(H) — same pattern as max path sum
```

---

## 58. Longest Path in DAG

```cpp
int longestPathDAG(int n, vector<vector<pair<int,int>>>& adj) {
    vector<int> indeg(n, 0);
    for (int u = 0; u < n; u++)
        for (auto& [v, w] : adj[u]) indeg[v]++;
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    vector<int> dp(n, 0);
    int best = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        best = max(best, dp[u]);
        for (auto& [v, w] : adj[u]) {
            dp[v] = max(dp[v], dp[u] + w);
            if (--indeg[v] == 0) q.push(v);
        }
    }
    return best;
}
// TC: O(V + E) | SC: O(V)
```

---

## 59. Number of Paths in DAG

```cpp
int countPathsDAG(int n, vector<vector<int>>& adj, int src, int dst) {
    vector<int> indeg(n, 0);
    for (int u = 0; u < n; u++) for (int v : adj[u]) indeg[v]++;
    // Topo + DP
    vector<int> topo;
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        topo.push_back(u);
        for (int v : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    vector<int> dp(n, 0);
    dp[src] = 1;
    for (int u : topo) {
        if (dp[u] == 0) continue;
        for (int v : adj[u]) dp[v] += dp[u];
    }
    return dp[dst];
}
// TC: O(V + E) | SC: O(V)
```

---

## 60. Cherry Pickup II — Two Robots on Grid

```cpp
int cherryPickupII(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<vector<int>>> dp(m + 1, vector<vector<int>>(n + 2, vector<int>(n + 2, -1)));
    dp[0][1][n] = grid[0][0] + (n > 1 ? grid[0][n-1] : 0);
    for (int i = 1; i < m; i++) {
        for (int c1 = 1; c1 <= n; c1++) {
            for (int c2 = 1; c2 <= n; c2++) {
                int best = -1;
                for (int dc1 = -1; dc1 <= 1; dc1++)
                    for (int dc2 = -1; dc2 <= 1; dc2++)
                        if (dp[i-1][c1+dc1][c2+dc2] >= 0)
                            best = max(best, dp[i-1][c1+dc1][c2+dc2]);
                if (best < 0) continue;
                int cherries = grid[i][c1-1] + (c1 != c2 ? grid[i][c2-1] : 0);
                dp[i][c1][c2] = best + cherries;
            }
        }
    }
    int ans = 0;
    for (int c1 = 1; c1 <= n; c1++)
        for (int c2 = 1; c2 <= n; c2++)
            ans = max(ans, dp[m-1][c1][c2]);
    return ans;
}
// TC: O(M × N²) | SC: O(M × N²)
```

---

# 🎭 BITMASK DP

---

## 61. Travelling Salesman (Bitmask DP)

```cpp
int tsp(int n, vector<vector<int>>& cost) {
    int FULL = (1 << n) - 1;
    vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX));
    dp[1][0] = 0;
    for (int mask = 1; mask <= FULL; mask++) {
        for (int u = 0; u < n; u++) {
            if (!(mask & (1 << u)) || dp[mask][u] == INT_MAX) continue;
            for (int v = 0; v < n; v++) {
                if (mask & (1 << v)) continue;
                int newMask = mask | (1 << v);
                dp[newMask][v] = min(dp[newMask][v], dp[mask][u] + cost[u][v]);
            }
        }
    }
    int ans = INT_MAX;
    for (int u = 1; u < n; u++)
        if (dp[FULL][u] != INT_MAX)
            ans = min(ans, dp[FULL][u] + cost[u][0]);
    return ans;
}
// TC: O(N² × 2ⁿ) | SC: O(N × 2ⁿ)
// Works for N ≤ 20.
```

---

## 62. Assignment Problem

**N people, N tasks, cost matrix. Min total cost for one-to-one assignment.**

```cpp
int assignmentMin(vector<vector<int>>& cost) {
    int n = cost.size();
    vector<int> dp(1 << n, INT_MAX);
    dp[0] = 0;
    for (int mask = 0; mask < (1 << n); mask++) {
        if (dp[mask] == INT_MAX) continue;
        int person = __builtin_popcount(mask);
        if (person == n) continue;
        for (int task = 0; task < n; task++) {
            if (mask & (1 << task)) continue;
            int newMask = mask | (1 << task);
            dp[newMask] = min(dp[newMask], dp[mask] + cost[person][task]);
        }
    }
    return dp[(1 << n) - 1];
}
// TC: O(N × 2ⁿ) | SC: O(2ⁿ)
```

---

## 63. Counting Subsets with Property (Sum of Subset / Sum-of-Subsets DP)

**Common pattern for "iterate over all subsets of a mask".**

```cpp
// For each mask, iterate over its submasks
for (int mask = 0; mask < (1 << n); mask++) {
    for (int sub = mask; sub > 0; sub = (sub - 1) & mask) {
        // sub is a non-empty submask of mask
    }
}
// Total work: O(3^n) — for each bit, choose: not in mask / in submask / in (mask \ submask)
```

---

# 🔴 MISCELLANEOUS HARD

---

## 64. Partition Equal Sum K Subsets

```cpp
bool canPartitionKSubsets(vector<int>& nums, int k) {
    int sum = accumulate(nums.begin(), nums.end(), 0);
    if (sum % k) return false;
    int target = sum / k, n = nums.size();
    sort(nums.rbegin(), nums.rend());
    if (nums[0] > target) return false;
    vector<int> buckets(k, 0);
    function<bool(int)> backtrack = [&](int idx) -> bool {
        if (idx == n) return true;
        for (int i = 0; i < k; i++) {
            if (buckets[i] + nums[idx] > target) continue;
            if (i > 0 && buckets[i] == buckets[i-1]) continue;     // prune symmetric
            buckets[i] += nums[idx];
            if (backtrack(idx + 1)) return true;
            buckets[i] -= nums[idx];
        }
        return false;
    };
    return backtrack(0);
}
// TC: O(K^N) worst, much faster with pruning
```

---

## 65. Min Refueling Stops

**Reach target with min refuel stops.** Heap-based DP.

```cpp
int minRefuelStops(int target, int start, vector<vector<int>>& stations) {
    priority_queue<int> pq;
    int stops = 0, prev = 0, fuel = start;
    for (auto& s : stations) {
        fuel -= s[0] - prev;
        while (!pq.empty() && fuel < 0) { fuel += pq.top(); pq.pop(); stops++; }
        if (fuel < 0) return -1;
        pq.push(s[1]);
        prev = s[0];
    }
    fuel -= target - prev;
    while (!pq.empty() && fuel < 0) { fuel += pq.top(); pq.pop(); stops++; }
    return fuel < 0 ? -1 : stops;
}
// TC: O(N log N) | SC: O(N)
```

---

## 66. Frog Jump (LeetCode 403)

```cpp
bool canCross(vector<int>& stones) {
    unordered_map<int, unordered_set<int>> dp;
    for (int s : stones) dp[s] = {};
    dp[0].insert(0);
    for (int s : stones) {
        for (int k : dp[s]) {
            for (int step : {k-1, k, k+1}) {
                if (step > 0 && dp.count(s + step)) dp[s + step].insert(step);
            }
        }
    }
    return !dp[stones.back()].empty();
}
// TC: O(N²) | SC: O(N²)
```

---

## 67. Stone Game (Game Theory DP)

**Two players take from either end of array. Determine winner.**

```cpp
bool stoneGame(vector<int>& piles) {
    int n = piles.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int i = 0; i < n; i++) dp[i][i] = piles[i];
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i + len <= n; i++) {
            int j = i + len - 1;
            dp[i][j] = max(piles[i] - dp[i+1][j], piles[j] - dp[i][j-1]);
        }
    }
    return dp[0][n-1] > 0;
}
// TC: O(N²) | SC: O(N²)
// dp[i][j] = best advantage current player can achieve on range [i, j]
```

---

## 68. Number of Longest Increasing Subsequences

```cpp
int findNumberOfLIS(vector<int>& nums) {
    int n = nums.size(), maxLen = 0, count = 0;
    vector<int> len(n, 1), cnt(n, 1);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                if (len[j] + 1 > len[i]) { len[i] = len[j] + 1; cnt[i] = cnt[j]; }
                else if (len[j] + 1 == len[i]) cnt[i] += cnt[j];
            }
        }
        if (len[i] > maxLen) { maxLen = len[i]; count = cnt[i]; }
        else if (len[i] == maxLen) count += cnt[i];
    }
    return count;
}
// TC: O(N²) | SC: O(N)
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Universal DP Templates

### Top-Down (Memoization)

```cpp
unordered_map<string,int> memo;
int solve(/* state */) {
    if (/* base case */) return /* base value */;
    string key = encode(state);
    if (memo.count(key)) return memo[key];
    int result = /* recurrence */;
    return memo[key] = result;
}
```

### Bottom-Up (Tabulation)

```cpp
vector<...> dp(/* dimensions */, /* initial */);
// Set base cases
for (/* state in correct order */) {
    dp[state] = /* recurrence using smaller states */;
}
return dp[/* final state */];
```

### 1D Optimization (only depend on previous)

```cpp
int prev = base, cur = base;
for (int i = 2; i <= n; i++) {
    int next = f(prev, cur);
    prev = cur; cur = next;
}
return cur;
```

### Knapsack 1D (iterate weight reverse for 0/1)

```cpp
vector<int> dp(W + 1, 0);
for (int i = 0; i < n; i++) {
    for (int w = W; w >= wt[i]; w--) {                  // REVERSE for 0/1
        dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
    }
}
```

### 2D String DP (LCS pattern)

```cpp
vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (a[i-1] == b[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
        else                  dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
    }
}
```

---

## ⭐ Top 15 Must-Know DP Problems

1. **Fibonacci / Climbing Stairs** — entry point
2. **House Robber I & II** — pick-or-skip pattern
3. **Maximum Subarray (Kadane's)** — running max trick
4. **Longest Increasing Subsequence** — both O(N²) and O(N log N)
5. **Unique Paths / Min Path Sum** — grid DP
6. **0/1 Knapsack** — foundation for many problems
7. **Coin Change** — min + count variants
8. **Longest Common Subsequence (LCS)** — base for string DP
9. **Edit Distance** — most asked string DP
10. **Longest Palindromic Substring/Subsequence**
11. **Word Break** — string + dictionary
12. **Stock Buy/Sell** — state machine
13. **Matrix Chain Multiplication / Burst Balloons** — interval DP
14. **Partition Equal Subset Sum** — boolean knapsack
15. **TSP (Bitmask DP)** — small N exponential DP

---

## ⭐ Common Interview Questions

> **Q: What is DP?**
> A: An optimization technique for problems with **optimal substructure** + **overlapping subproblems**. Cache results to avoid recomputation, reducing exponential brute force to polynomial.

> **Q: Memoization vs Tabulation?**
> A: Memoization = top-down recursion + cache. Tabulation = bottom-up iteration filling a table. Memoization is easier to write; tabulation is faster (no recursion overhead) and easier to space-optimize.

> **Q: How do you identify a DP problem?**
> A: Look for: (1) optimization keywords (min/max/count), (2) decisions at each step, (3) future depends on past, (4) brute force has overlapping subproblems.

> **Q: 0/1 vs Unbounded Knapsack — code difference?**
> A: In the 1D-optimized loop, **0/1 iterates weight in reverse**, unbounded iterates **forward**. Reverse prevents reusing the item; forward allows reusing.

> **Q: Why does Coin Change II coin loop go OUTSIDE?**
> A: To count **combinations** (order doesn't matter). Putting amount outside counts **permutations** ({1,2,1} vs {1,1,2}).

> **Q: How to space-optimize a 2D DP?**
> A: If `dp[i][j]` only depends on row `i-1`, keep only one row. Iterate j carefully — sometimes need to save the diagonal value before overwriting.

> **Q: Common DP order pitfall?**
> A: Filling table in wrong order — referenced subproblem not computed yet. Always derive recurrence first, then write loops.

> **Q: What's optimal substructure?**
> A: Optimal solution to problem is composed of optimal solutions to subproblems. Required for DP/greedy to work. Counter-example: longest simple path in general graph (NP-hard).

> **Q: How to reconstruct the actual solution?**
> A: Store the choice at each state OR backtrack through the DP table after filling (e.g., reconstruct LCS by following `dp[i][j]`).

> **Q: When use bitmask DP?**
> A: When state involves "which subset of N items is used", and N is small (≤ 20). Examples: TSP, assignment, subset enumeration.

> **Q: Difference between subsequence and substring?**
> A: **Substring** = contiguous. **Subsequence** = relative order preserved but not contiguous. Different recurrences!

> **Q: Why is LIS O(N log N) possible?**
> A: Maintain `tails[i]` = smallest tail of any LIS of length `i+1`. Binary search to update — gives the length in O(N log N), though not the actual LIS.

> **Q: How to detect cycle of subproblems in DP?**
> A: If recursion can revisit the same state via a cycle, DP doesn't apply. Need to verify DAG structure on state graph.

---

## ⭐ Common Pitfalls

✅ **Off-by-one errors** — use `dp[i+1]` if dp is sized `n+1` and `i` is 0-indexed.
✅ **Wrong loop direction** for space-optimized knapsack — reverse for 0/1, forward for unbounded.
✅ **Missing base case** — `dp[0]` or `dp[0][j]`/`dp[i][0]`.
✅ **Integer overflow** — large counts, multiplications. Use `long long`.
✅ **Wrong state definition** — try to express "what info do I need to make next decision?"
✅ **Substring ≠ subsequence** — different recurrences. Substring resets on mismatch.
✅ **Modifying state during iteration** — copy if needed.
✅ **`INT_MAX` arithmetic** — `INT_MAX + 1` overflows. Check before adding.
✅ **Coin Change loop order** — combinations vs permutations.
✅ **Recursion depth** — for huge N, stack overflow in memoization. Convert to tabulation.
✅ **Top-down without proper memo key** — encode all state fields.
✅ **Comparator pitfall in DP-with-sorting** — strict weak ordering.

---

## ⭐ DP Decision Tree

```
Optimization problem (min / max / count / can)?
│
├─ Single dimension, each element decided once?
│  └─ 1D linear DP (House Robber, LIS, Kadane)
│
├─ Two sequences / 2D grid?
│  └─ 2D DP (LCS, Edit Distance, Unique Paths)
│
├─ "Pick subset with constraints"?
│  └─ Knapsack family (0/1, unbounded, subset sum)
│
├─ "Partition / merge / interval"?
│  └─ Interval DP (Matrix Chain, Burst Balloons)
│
├─ "Visit subset of N (≤ 20) items"?
│  └─ Bitmask DP (TSP, assignment)
│
├─ "Tree / Subtree"?
│  └─ Tree DP via DFS returning multiple values
│
└─ "Optimal play / Min-max"?
   └─ Game theory DP (Stone Game, Minimax)
```

---

## ⭐ Practice Problems by Pattern

| LeetCode | Pattern |
|----------|---------|
| 70, 198, 213, 740 | 1D linear |
| 53, 152 | Kadane variants |
| 300, 354, 673 | LIS family |
| 62, 63, 64, 120, 174 | 2D grid |
| 221, 85 | Maximal square / rectangle |
| 416, 494, 474 | Subset sum / partition |
| 322, 518, 377 | Coin change |
| 1143, 583, 72 | LCS / Edit distance |
| 5, 516, 647, 132 | Palindromes |
| 121–123, 188, 309, 714 | Stock series |
| 312, 1547, 1039 | Interval / matrix chain |
| 337 | Tree DP |
| 1335, 1216 | Hard DP |
| 691, 698, 691 | Bitmask DP |
| 887 | Egg drop |
| 10, 44 | Regex / wildcard |
| 32 | Longest valid parens |
| 91, 639 | Decode ways |
| 139, 140 | Word break |
| 264, 313 | Ugly number / super ugly |

---

# 💪 GO ACE THAT DP QUESTION!

> **Test-day strategy:**
> 1. **Identify the pattern** first — pattern recognition is 80% of DP.
> 2. **Define state in words** — "dp[i] = ..." — clarity prevents bugs.
> 3. **Write recurrence** — relate to smaller subproblems.
> 4. **Verify with small example** — N=3 or 4 by hand.
> 5. **Start with memoization** — easier to write, easy to debug.
> 6. **Optimize space only if needed** — readability matters in interviews.
> 7. **Always state base cases explicitly** in code AND when explaining.
> 8. **Reconstruct if asked** — store choices or backtrack the table.
>
> **You've got this! 🧠🚀**
