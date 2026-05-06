# 🗝️ Complete Hashing / Map / Set Interview Sheet — C++

> **The ultimate Hashing, HashMap, HashSet, Map & Set guide for coding interviews.**
> Covers `unordered_map`, `unordered_set`, `map`, `set`, `multiset`, `multimap` — every must-know problem.

---

## 📑 Table of Contents

**FOUNDATIONS (1–10)**
1. STL Map / Set Quick Reference (with complexities)
2. Frequency of Each Element
3. Highest & Lowest Frequency Element
4. Check if Two Arrays are Equal (multiset equality)
5. Find First Repeating Element
6. Find First Non-Repeating Element
7. Check Duplicate Exists in Array
8. Find All Duplicates
9. Find All Distinct Elements
10. Count Distinct Elements in Every Window of Size K

**EASY (11–25)**
11. Two Sum (Hashing)
12. Contains Duplicate within K Distance
13. Intersection of Two Arrays (using sets)
14. Union of Two Arrays (using sets)
15. Find Missing Number Using Hashing
16. Subarray with Given Sum (positives + negatives)
17. Count Subarrays with Sum K
18. Longest Subarray with Sum K
19. Largest Subarray with Equal 0s and 1s
20. Subarray with 0 Sum (Existence)
21. Check Anagram via HashMap
22. Group Anagrams
23. Common Elements in Three Sorted Arrays
24. Longest Consecutive Sequence
25. Top K Frequent Elements

**MEDIUM (26–42)**
26. Subarrays with XOR K
27. Count Subarrays with Equal Number of 0s and 1s
28. 4Sum with Hashing (4Sum II — count pairs)
29. Isomorphic Strings
30. Word Pattern
31. Valid Sudoku
32. Longest Substring Without Repeating Chars
33. Minimum Window Substring
34. Find All Anagrams in a String
35. Permutation in String (sliding window + freq map)
36. Continuous Subarray Sum (multiple of K)
37. Maximum Points You Can Obtain (using prefix counts)
38. Smallest Window Containing All Distinct Chars
39. Group Strings by Shifting Pattern
40. Longest Subarray with Sum Divisible by K
41. Pairs Whose Sum is Divisible by K
42. Brick Wall (least bricks crossed)

**HARD (43–55)**
43. LRU Cache (HashMap + Doubly Linked List)
44. LFU Cache
45. Insert / Delete / GetRandom O(1)
46. Insert / Delete / GetRandom with Duplicates O(1)
47. Subarrays with K Different Integers
48. Number of Distinct Substrings
49. Longest Duplicate Substring (Hashing + Binary Search)
50. Two Sum III — Data Structure Design
51. All O(1) Data Structure
52. Encode and Decode TinyURL
53. Time-Based Key-Value Store
54. Snapshot Array
55. Design HashMap from Scratch

---

## ⚡ STL Quick Complexity Reference

| Container | Lookup | Insert | Delete | Ordered? | Backed By |
|-----------|--------|--------|--------|----------|-----------|
| `unordered_map` | O(1) avg / O(N) worst | O(1) avg | O(1) avg | ❌ | Hash table |
| `unordered_set` | O(1) avg / O(N) worst | O(1) avg | O(1) avg | ❌ | Hash table |
| `map` | O(log N) | O(log N) | O(log N) | ✅ | Red-Black tree |
| `set` | O(log N) | O(log N) | O(log N) | ✅ | Red-Black tree |
| `multiset` / `multimap` | O(log N) | O(log N) | O(log N) | ✅ | Allows duplicates |

**Rule of thumb:**
- Need order? → `map` / `set`
- Just lookup? → `unordered_map` / `unordered_set`
- Allow duplicates? → `multiset` / `multimap` or `unordered_map<key, count>`

---

# 🟢 FOUNDATIONS

---

## 1. STL Map / Set Quick Reference

```cpp
#include <bits/stdc++.h>
using namespace std;

// ----- unordered_map -----
unordered_map<string, int> ump;
ump["apple"] = 5;
ump.insert({"banana", 3});
if (ump.count("apple")) cout << ump["apple"];
if (ump.find("kiwi") != ump.end()) cout << "found";
ump.erase("apple");
for (auto& [k, v] : ump) cout << k << ":" << v << "\n";

// ----- unordered_set -----
unordered_set<int> uset = {1, 2, 3};
uset.insert(4);
if (uset.count(2)) cout << "exists";
uset.erase(1);

// ----- map (ordered) -----
map<int, string> mp;
mp[1] = "one";
mp.lower_bound(2);   // first key >= 2
mp.upper_bound(2);   // first key >  2
mp.begin()->first;   // smallest key
mp.rbegin()->first;  // largest key

// ----- set (ordered) -----
set<int> s = {3, 1, 4, 1, 5};   // {1, 3, 4, 5}
s.insert(9);
auto it = s.find(3);

// ----- multiset (duplicates allowed) -----
multiset<int> ms = {1, 1, 2, 3};
ms.insert(1);
ms.erase(ms.find(1));   // erase ONE occurrence (NOT ms.erase(1) which erases ALL)
cout << ms.count(1);    // count occurrences

// ----- multimap -----
multimap<string, int> mm;
mm.insert({"a", 1});
mm.insert({"a", 2});
auto range = mm.equal_range("a");  // all entries for key "a"
```

---

## 2. Frequency of Each Element

```cpp
unordered_map<int, int> frequency(vector<int>& arr) {
    unordered_map<int, int> freq;
    for (int x : arr) freq[x]++;
    return freq;
}
// TC: O(N) avg | SC: O(N)
```

---

## 3. Highest & Lowest Frequency Element

```cpp
pair<int, int> highLowFreq(vector<int>& arr) {
    unordered_map<int, int> freq;
    for (int x : arr) freq[x]++;
    int maxFreq = 0, minFreq = INT_MAX;
    int maxEl = arr[0], minEl = arr[0];
    for (auto& [k, v] : freq) {
        if (v > maxFreq) { maxFreq = v; maxEl = k; }
        if (v < minFreq) { minFreq = v; minEl = k; }
    }
    return {maxEl, minEl};
}
// TC: O(N) avg | SC: O(N)
```

---

## 4. Check if Two Arrays are Equal (Multiset Equality)

```cpp
bool checkEqual(vector<int>& a, vector<int>& b) {
    if (a.size() != b.size()) return false;
    unordered_map<int, int> freq;
    for (int x : a) freq[x]++;
    for (int x : b) {
        if (--freq[x] < 0) return false;
    }
    return true;
}
// TC: O(N) avg | SC: O(N)
```

---

## 5. Find First Repeating Element

```cpp
int firstRepeating(vector<int>& arr) {
    unordered_map<int, int> count;
    for (int x : arr) count[x]++;
    for (int x : arr) {
        if (count[x] > 1) return x;
    }
    return -1;
}
// TC: O(N) avg | SC: O(N)
```

---

## 6. Find First Non-Repeating Element

```cpp
int firstNonRepeating(vector<int>& arr) {
    unordered_map<int, int> count;
    for (int x : arr) count[x]++;
    for (int x : arr) {
        if (count[x] == 1) return x;
    }
    return -1;
}
// TC: O(N) avg | SC: O(N)
```

---

## 7. Check Duplicate Exists in Array

```cpp
bool hasDuplicate(vector<int>& arr) {
    unordered_set<int> seen;
    for (int x : arr) {
        if (seen.count(x)) return true;
        seen.insert(x);
    }
    return false;
}
// TC: O(N) avg | SC: O(N)
```

---

## 8. Find All Duplicates

```cpp
vector<int> findDuplicates(vector<int>& arr) {
    unordered_map<int, int> count;
    vector<int> result;
    for (int x : arr) {
        if (++count[x] == 2) result.push_back(x);
    }
    return result;
}
// TC: O(N) avg | SC: O(N)
```

---

## 9. Find All Distinct Elements

```cpp
vector<int> findDistinct(vector<int>& arr) {
    unordered_set<int> seen;
    vector<int> result;
    for (int x : arr) {
        if (seen.insert(x).second) result.push_back(x);
    }
    return result;
}
// TC: O(N) avg | SC: O(N) — preserves first-occurrence order
```

---

## 10. Count Distinct Elements in Every Window of Size K

```cpp
vector<int> distinctInWindows(vector<int>& arr, int k) {
    vector<int> result;
    unordered_map<int, int> freq;
    int n = arr.size();
    for (int i = 0; i < k; i++) freq[arr[i]]++;
    result.push_back((int)freq.size());
    for (int i = k; i < n; i++) {
        freq[arr[i]]++;
        if (--freq[arr[i - k]] == 0) freq.erase(arr[i - k]);
        result.push_back((int)freq.size());
    }
    return result;
}
// TC: O(N) avg | SC: O(K)
```

---

# 🟢 EASY HASHING PROBLEMS

---

## 11. Two Sum (Hashing)

```cpp
vector<int> twoSum(vector<int>& arr, int target) {
    unordered_map<int, int> seen;
    for (int i = 0; i < arr.size(); i++) {
        int need = target - arr[i];
        if (seen.count(need)) return {seen[need], i};
        seen[arr[i]] = i;
    }
    return {-1, -1};
}
// TC: O(N) avg | SC: O(N)
```

---

## 12. Contains Duplicate within K Distance

**Return true if `arr[i] == arr[j]` and `|i - j| ≤ k`.**

```cpp
bool containsNearbyDuplicate(vector<int>& arr, int k) {
    unordered_map<int, int> lastIdx;
    for (int i = 0; i < arr.size(); i++) {
        if (lastIdx.count(arr[i]) && i - lastIdx[arr[i]] <= k) return true;
        lastIdx[arr[i]] = i;
    }
    return false;
}
// TC: O(N) avg | SC: O(N)
```

---

## 13. Intersection of Two Arrays

```cpp
vector<int> intersection(vector<int>& a, vector<int>& b) {
    unordered_set<int> sa(a.begin(), a.end());
    unordered_set<int> result;
    for (int x : b) if (sa.count(x)) result.insert(x);
    return vector<int>(result.begin(), result.end());
}
// TC: O(N + M) avg | SC: O(N)

// With duplicates (intersection counts) — LeetCode 350
vector<int> intersectWithCount(vector<int>& a, vector<int>& b) {
    unordered_map<int, int> freq;
    for (int x : a) freq[x]++;
    vector<int> result;
    for (int x : b) {
        if (freq[x] > 0) {
            result.push_back(x);
            freq[x]--;
        }
    }
    return result;
}
```

---

## 14. Union of Two Arrays

```cpp
vector<int> unionArrays(vector<int>& a, vector<int>& b) {
    unordered_set<int> s(a.begin(), a.end());
    s.insert(b.begin(), b.end());
    return vector<int>(s.begin(), s.end());
}
// TC: O(N + M) avg | SC: O(N + M)
```

---

## 15. Find Missing Number Using Hashing

```cpp
int missingNumber(vector<int>& arr, int n) {
    unordered_set<int> s(arr.begin(), arr.end());
    for (int i = 1; i <= n; i++) {
        if (!s.count(i)) return i;
    }
    return -1;
}
// TC: O(N) avg | SC: O(N)
// Note: O(1) space using sum/XOR is better — see arrays sheet
```

---

## 16. Subarray with Given Sum (positives + negatives)

**Return true if any contiguous subarray sums to k.**

```cpp
bool subarrayWithSumExists(vector<int>& arr, int k) {
    unordered_set<int> prefixes;
    prefixes.insert(0);
    int sum = 0;
    for (int x : arr) {
        sum += x;
        if (prefixes.count(sum - k)) return true;
        prefixes.insert(sum);
    }
    return false;
}
// TC: O(N) avg | SC: O(N)
```

---

## 17. Count Subarrays with Sum K

```cpp
int countSubarraysSumK(vector<int>& arr, int k) {
    unordered_map<int, int> prefixCount;
    prefixCount[0] = 1;
    int sum = 0, count = 0;
    for (int x : arr) {
        sum += x;
        if (prefixCount.count(sum - k)) count += prefixCount[sum - k];
        prefixCount[sum]++;
    }
    return count;
}
// TC: O(N) avg | SC: O(N)
```

---

## 18. Longest Subarray with Sum K

```cpp
int longestSubarraySumK(vector<int>& arr, int k) {
    unordered_map<int, int> firstIdx;
    int sum = 0, maxLen = 0;
    for (int i = 0; i < arr.size(); i++) {
        sum += arr[i];
        if (sum == k) maxLen = i + 1;
        if (firstIdx.count(sum - k))
            maxLen = max(maxLen, i - firstIdx[sum - k]);
        if (!firstIdx.count(sum)) firstIdx[sum] = i;  // store FIRST occurrence
    }
    return maxLen;
}
// TC: O(N) avg | SC: O(N)
```

---

## 19. Largest Subarray with Equal 0s and 1s

**Trick:** Replace 0 → −1, then find longest subarray with sum 0.

```cpp
int largestEqualBinary(vector<int>& arr) {
    unordered_map<int, int> firstIdx;
    int sum = 0, maxLen = 0;
    firstIdx[0] = -1;  // sentinel
    for (int i = 0; i < arr.size(); i++) {
        sum += (arr[i] == 0) ? -1 : 1;
        if (firstIdx.count(sum))
            maxLen = max(maxLen, i - firstIdx[sum]);
        else firstIdx[sum] = i;
    }
    return maxLen;
}
// TC: O(N) avg | SC: O(N)
```

---

## 20. Subarray with 0 Sum (Existence)

```cpp
bool hasZeroSumSubarray(vector<int>& arr) {
    unordered_set<int> prefixes;
    prefixes.insert(0);
    int sum = 0;
    for (int x : arr) {
        sum += x;
        if (prefixes.count(sum)) return true;
        prefixes.insert(sum);
    }
    return false;
}
// TC: O(N) avg | SC: O(N)
```

---

## 21. Check Anagram via HashMap

```cpp
bool isAnagram(string s, string t) {
    if (s.size() != t.size()) return false;
    unordered_map<char, int> freq;
    for (char c : s) freq[c]++;
    for (char c : t) {
        if (--freq[c] < 0) return false;
    }
    return true;
}
// TC: O(N) avg | SC: O(K) — K = unique chars
```

---

## 22. Group Anagrams

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for (string& s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        mp[key].push_back(s);
    }
    vector<vector<string>> result;
    for (auto& [k, v] : mp) result.push_back(v);
    return result;
}
// TC: O(N × K log K) avg | SC: O(N × K)
```

---

## 23. Common Elements in Three Sorted Arrays

```cpp
vector<int> commonElements(vector<int>& a, vector<int>& b, vector<int>& c) {
    unordered_map<int, int> freq;
    for (int x : a) freq[x] = 1;
    for (int x : b) if (freq[x] == 1) freq[x] = 2;
    vector<int> result;
    for (int x : c) {
        if (freq[x] == 2) {
            result.push_back(x);
            freq[x] = 3;  // avoid duplicates in c
        }
    }
    return result;
}
// TC: O(N + M + L) avg | SC: O(N)

// Three-pointer alternative (uses sortedness, O(1) extra) — see arrays sheet
```

---

## 24. Longest Consecutive Sequence

```cpp
int longestConsecutive(vector<int>& arr) {
    unordered_set<int> s(arr.begin(), arr.end());
    int longest = 0;
    for (int x : s) {
        if (s.count(x - 1)) continue;          // not a sequence start
        int cur = x, len = 1;
        while (s.count(cur + 1)) { cur++; len++; }
        longest = max(longest, len);
    }
    return longest;
}
// TC: O(N) avg | SC: O(N)
```

---

## 25. Top K Frequent Elements

```cpp
vector<int> topKFrequent(vector<int>& arr, int k) {
    unordered_map<int, int> freq;
    for (int x : arr) freq[x]++;
    // Min-heap of size k, ordered by frequency
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    for (auto& [num, f] : freq) {
        pq.push({f, num});
        if (pq.size() > k) pq.pop();
    }
    vector<int> result;
    while (!pq.empty()) { result.push_back(pq.top().second); pq.pop(); }
    return result;
}
// TC: O(N log K) | SC: O(N + K)

// Bucket-sort approach — O(N)
vector<int> topKBucket(vector<int>& arr, int k) {
    unordered_map<int, int> freq;
    for (int x : arr) freq[x]++;
    vector<vector<int>> buckets(arr.size() + 1);
    for (auto& [num, f] : freq) buckets[f].push_back(num);
    vector<int> result;
    for (int i = buckets.size() - 1; i >= 0 && result.size() < k; i--) {
        for (int x : buckets[i]) {
            result.push_back(x);
            if (result.size() == k) break;
        }
    }
    return result;
}
```

---

# 🟡 MEDIUM HASHING PROBLEMS

---

## 26. Subarrays with XOR K

```cpp
int subarraysXOR(vector<int>& arr, int k) {
    unordered_map<int, int> mp;
    mp[0] = 1;
    int xorSum = 0, count = 0;
    for (int x : arr) {
        xorSum ^= x;
        int need = xorSum ^ k;
        if (mp.count(need)) count += mp[need];
        mp[xorSum]++;
    }
    return count;
}
// TC: O(N) avg | SC: O(N)
```

---

## 27. Count Subarrays with Equal Number of 0s and 1s

```cpp
int countEqual01(vector<int>& arr) {
    unordered_map<int, int> prefixCount;
    prefixCount[0] = 1;
    int sum = 0, count = 0;
    for (int x : arr) {
        sum += (x == 0) ? -1 : 1;
        if (prefixCount.count(sum)) count += prefixCount[sum];
        prefixCount[sum]++;
    }
    return count;
}
// TC: O(N) avg | SC: O(N)
```

---

## 28. 4Sum II — Count Tuples a+b+c+d == 0

```cpp
int fourSumCount(vector<int>& A, vector<int>& B,
                 vector<int>& C, vector<int>& D) {
    unordered_map<int, int> sumAB;
    for (int a : A)
        for (int b : B)
            sumAB[a + b]++;
    int count = 0;
    for (int c : C)
        for (int d : D)
            count += sumAB[-(c + d)];
    return count;
}
// TC: O(N²) | SC: O(N²) — much better than O(N⁴)
```

---

## 29. Isomorphic Strings

**`s` and `t` are isomorphic if chars in `s` can be replaced to get `t` (1-to-1 mapping).**

```cpp
bool isIsomorphic(string s, string t) {
    if (s.size() != t.size()) return false;
    unordered_map<char, char> mST, mTS;
    for (int i = 0; i < s.size(); i++) {
        char a = s[i], b = t[i];
        if (mST.count(a) && mST[a] != b) return false;
        if (mTS.count(b) && mTS[b] != a) return false;
        mST[a] = b;
        mTS[b] = a;
    }
    return true;
}
// TC: O(N) avg | SC: O(K)
```

---

## 30. Word Pattern

**Pattern `"abba"`, string `"dog cat cat dog"` → true.**

```cpp
bool wordPattern(string pattern, string s) {
    vector<string> words;
    stringstream ss(s);
    string w;
    while (ss >> w) words.push_back(w);
    if (words.size() != pattern.size()) return false;
    unordered_map<char, string> cToW;
    unordered_map<string, char> wToC;
    for (int i = 0; i < pattern.size(); i++) {
        char c = pattern[i];
        if (cToW.count(c) && cToW[c] != words[i]) return false;
        if (wToC.count(words[i]) && wToC[words[i]] != c) return false;
        cToW[c] = words[i];
        wToC[words[i]] = c;
    }
    return true;
}
// TC: O(N) avg | SC: O(N)
```

---

## 31. Valid Sudoku

```cpp
bool isValidSudoku(vector<vector<char>>& board) {
    unordered_set<string> seen;
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            char c = board[i][j];
            if (c == '.') continue;
            string row = "r" + to_string(i) + c;
            string col = "c" + to_string(j) + c;
            string box = "b" + to_string(i/3) + to_string(j/3) + c;
            if (!seen.insert(row).second) return false;
            if (!seen.insert(col).second) return false;
            if (!seen.insert(box).second) return false;
        }
    }
    return true;
}
// TC: O(81) = O(1) | SC: O(1)
```

---

## 32. Longest Substring Without Repeating Chars

```cpp
int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> lastIdx;
    int left = 0, maxLen = 0;
    for (int right = 0; right < s.size(); right++) {
        if (lastIdx.count(s[right]) && lastIdx[s[right]] >= left)
            left = lastIdx[s[right]] + 1;
        lastIdx[s[right]] = right;
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
// TC: O(N) avg | SC: O(K) — K = alphabet size
```

---

## 33. Minimum Window Substring

```cpp
string minWindow(string s, string t) {
    if (s.size() < t.size()) return "";
    unordered_map<char, int> need;
    for (char c : t) need[c]++;
    int required = need.size(), formed = 0;
    unordered_map<char, int> windowCount;
    int left = 0, minLen = INT_MAX, minStart = 0;
    for (int right = 0; right < s.size(); right++) {
        char c = s[right];
        windowCount[c]++;
        if (need.count(c) && windowCount[c] == need[c]) formed++;
        while (formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minStart = left;
            }
            char lc = s[left];
            windowCount[lc]--;
            if (need.count(lc) && windowCount[lc] < need[lc]) formed--;
            left++;
        }
    }
    return minLen == INT_MAX ? "" : s.substr(minStart, minLen);
}
// TC: O(N + M) avg | SC: O(K)
```

---

## 34. Find All Anagrams in a String

```cpp
vector<int> findAnagrams(string s, string p) {
    vector<int> result;
    if (s.size() < p.size()) return result;
    int need[26] = {0}, have[26] = {0};
    for (char c : p) need[c - 'a']++;
    int k = p.size();
    for (int i = 0; i < s.size(); i++) {
        have[s[i] - 'a']++;
        if (i >= k) have[s[i - k] - 'a']--;
        if (i >= k - 1 && memcmp(have, need, sizeof(have)) == 0)
            result.push_back(i - k + 1);
    }
    return result;
}
// TC: O(N) | SC: O(1)
```

---

## 35. Permutation in String

**Return true if any permutation of `s1` is a substring of `s2`.**

```cpp
bool checkInclusion(string s1, string s2) {
    if (s1.size() > s2.size()) return false;
    int need[26] = {0}, have[26] = {0};
    for (char c : s1) need[c - 'a']++;
    int k = s1.size();
    for (int i = 0; i < s2.size(); i++) {
        have[s2[i] - 'a']++;
        if (i >= k) have[s2[i - k] - 'a']--;
        if (i >= k - 1 && memcmp(have, need, sizeof(have)) == 0)
            return true;
    }
    return false;
}
// TC: O(N) | SC: O(1)
```

---

## 36. Continuous Subarray Sum (Multiple of K)

**Return true if a subarray of size ≥ 2 sums to a multiple of k.**

```cpp
bool checkSubarraySum(vector<int>& arr, int k) {
    unordered_map<int, int> remIdx;
    remIdx[0] = -1;
    int sum = 0;
    for (int i = 0; i < arr.size(); i++) {
        sum += arr[i];
        int rem = (k == 0) ? sum : sum % k;
        if (remIdx.count(rem)) {
            if (i - remIdx[rem] >= 2) return true;
        } else remIdx[rem] = i;
    }
    return false;
}
// TC: O(N) avg | SC: O(N)
// Idea: same prefix-sum remainder mod k → subarray between is divisible by k
```

---

## 37. Maximum Points You Can Obtain (LeetCode 1423)

**Take exactly k cards from either end; maximize sum.**

```cpp
int maxScore(vector<int>& cards, int k) {
    int n = cards.size();
    int leftSum = 0;
    for (int i = 0; i < k; i++) leftSum += cards[i];
    int maxSum = leftSum, rightSum = 0;
    for (int i = 0; i < k; i++) {
        leftSum -= cards[k - 1 - i];
        rightSum += cards[n - 1 - i];
        maxSum = max(maxSum, leftSum + rightSum);
    }
    return maxSum;
}
// TC: O(K) | SC: O(1) — frequency-style sliding logic
```

---

## 38. Smallest Window Containing All Distinct Chars of String

```cpp
string smallestWindowAllDistinct(string s) {
    unordered_set<char> distinct(s.begin(), s.end());
    int required = distinct.size();
    unordered_map<char, int> count;
    int left = 0, formed = 0, minLen = INT_MAX, minStart = 0;
    for (int right = 0; right < s.size(); right++) {
        if (++count[s[right]] == 1) formed++;
        while (formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minStart = left;
            }
            if (--count[s[left]] == 0) formed--;
            left++;
        }
    }
    return s.substr(minStart, minLen);
}
// TC: O(N) avg | SC: O(K)
```

---

## 39. Group Strings by Shifting Pattern

**`"abc"`, `"bcd"`, `"xyz"` belong to the same group (each char shifted by same amount).**

```cpp
vector<vector<string>> groupShifted(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for (string& s : strs) {
        string key;
        for (int i = 0; i < s.size(); i++) {
            int diff = (s[i] - s[0] + 26) % 26;
            key += to_string(diff) + ",";
        }
        mp[key].push_back(s);
    }
    vector<vector<string>> result;
    for (auto& [k, v] : mp) result.push_back(v);
    return result;
}
// TC: O(N × K) avg | SC: O(N × K)
```

---

## 40. Longest Subarray with Sum Divisible by K

```cpp
int longestSubarrayDivK(vector<int>& arr, int k) {
    unordered_map<int, int> firstIdx;
    firstIdx[0] = -1;
    int sum = 0, maxLen = 0;
    for (int i = 0; i < arr.size(); i++) {
        sum += arr[i];
        int rem = ((sum % k) + k) % k;
        if (firstIdx.count(rem))
            maxLen = max(maxLen, i - firstIdx[rem]);
        else firstIdx[rem] = i;
    }
    return maxLen;
}
// TC: O(N) avg | SC: O(K)
```

---

## 41. Pairs Whose Sum is Divisible by K

```cpp
int countPairsDivK(vector<int>& arr, int k) {
    vector<int> remCount(k, 0);
    for (int x : arr) remCount[((x % k) + k) % k]++;
    long long count = (long long)remCount[0] * (remCount[0] - 1) / 2;
    for (int i = 1; i <= k / 2; i++) {
        if (i != k - i)
            count += (long long)remCount[i] * remCount[k - i];
        else
            count += (long long)remCount[i] * (remCount[i] - 1) / 2;
    }
    return (int)count;
}
// TC: O(N + K) | SC: O(K)
```

---

## 42. Brick Wall — Least Bricks Crossed

**Find vertical line crossing fewest bricks.** (LeetCode 554)

```cpp
int leastBricks(vector<vector<int>>& wall) {
    unordered_map<int, int> edgeCount;
    int maxEdges = 0;
    for (auto& row : wall) {
        int pos = 0;
        for (int i = 0; i < row.size() - 1; i++) {  // skip last edge (wall end)
            pos += row[i];
            maxEdges = max(maxEdges, ++edgeCount[pos]);
        }
    }
    return (int)wall.size() - maxEdges;
}
// TC: O(total bricks) | SC: O(unique edges)
```

---

# 🔴 HARD HASHING / DESIGN PROBLEMS

---

## 43. LRU Cache — HashMap + Doubly Linked List

```cpp
class LRUCache {
    int cap;
    list<pair<int, int>> dll;  // {key, value}
    unordered_map<int, list<pair<int,int>>::iterator> mp;
public:
    LRUCache(int capacity) : cap(capacity) {}

    int get(int key) {
        if (!mp.count(key)) return -1;
        dll.splice(dll.begin(), dll, mp[key]);  // move to front
        return mp[key]->second;
    }

    void put(int key, int value) {
        if (mp.count(key)) {
            mp[key]->second = value;
            dll.splice(dll.begin(), dll, mp[key]);
            return;
        }
        if (dll.size() == cap) {
            mp.erase(dll.back().first);
            dll.pop_back();
        }
        dll.push_front({key, value});
        mp[key] = dll.begin();
    }
};
// TC: O(1) per op | SC: O(capacity)
```

---

## 44. LFU Cache

```cpp
class LFUCache {
    int cap, minFreq;
    unordered_map<int, pair<int, int>> kv;            // key -> {value, freq}
    unordered_map<int, list<int>::iterator> kIter;     // key -> iterator in freq list
    unordered_map<int, list<int>> freqList;            // freq -> list of keys
public:
    LFUCache(int capacity) : cap(capacity), minFreq(0) {}

    int get(int key) {
        if (!kv.count(key)) return -1;
        int f = kv[key].second;
        freqList[f].erase(kIter[key]);
        if (freqList[f].empty()) {
            freqList.erase(f);
            if (minFreq == f) minFreq++;
        }
        f++;
        kv[key].second = f;
        freqList[f].push_front(key);
        kIter[key] = freqList[f].begin();
        return kv[key].first;
    }

    void put(int key, int value) {
        if (cap <= 0) return;
        if (kv.count(key)) {
            kv[key].first = value;
            get(key);  // bump freq
            return;
        }
        if (kv.size() == cap) {
            int evict = freqList[minFreq].back();
            freqList[minFreq].pop_back();
            if (freqList[minFreq].empty()) freqList.erase(minFreq);
            kv.erase(evict);
            kIter.erase(evict);
        }
        kv[key] = {value, 1};
        freqList[1].push_front(key);
        kIter[key] = freqList[1].begin();
        minFreq = 1;
    }
};
// TC: O(1) per op | SC: O(capacity)
```

---

## 45. Insert / Delete / GetRandom O(1)

```cpp
class RandomizedSet {
    vector<int> arr;
    unordered_map<int, int> idx;
public:
    bool insert(int val) {
        if (idx.count(val)) return false;
        idx[val] = arr.size();
        arr.push_back(val);
        return true;
    }

    bool remove(int val) {
        if (!idx.count(val)) return false;
        int i = idx[val], last = arr.back();
        arr[i] = last;
        idx[last] = i;
        arr.pop_back();
        idx.erase(val);
        return true;
    }

    int getRandom() {
        return arr[rand() % arr.size()];
    }
};
// TC: O(1) avg per op | SC: O(N)
```

---

## 46. Insert / Delete / GetRandom with Duplicates O(1)

```cpp
class RandomizedCollection {
    vector<int> arr;
    unordered_map<int, unordered_set<int>> idx;
public:
    bool insert(int val) {
        bool isNew = !idx.count(val) || idx[val].empty();
        idx[val].insert(arr.size());
        arr.push_back(val);
        return isNew;
    }

    bool remove(int val) {
        if (!idx.count(val) || idx[val].empty()) return false;
        int i = *idx[val].begin();
        idx[val].erase(i);
        int last = arr.back();
        arr[i] = last;
        if (i != arr.size() - 1) {
            idx[last].erase(arr.size() - 1);
            idx[last].insert(i);
        }
        arr.pop_back();
        return true;
    }

    int getRandom() {
        return arr[rand() % arr.size()];
    }
};
// TC: O(1) avg per op | SC: O(N)
```

---

## 47. Subarrays with K Different Integers

**Trick:** `exactly(K) = atMost(K) - atMost(K-1)`.

```cpp
int atMostK(vector<int>& arr, int k) {
    unordered_map<int, int> count;
    int left = 0, result = 0;
    for (int right = 0; right < arr.size(); right++) {
        if (++count[arr[right]] == 1) k--;
        while (k < 0) {
            if (--count[arr[left]] == 0) k++;
            left++;
        }
        result += right - left + 1;
    }
    return result;
}

int subarraysWithKDistinct(vector<int>& arr, int k) {
    return atMostK(arr, k) - atMostK(arr, k - 1);
}
// TC: O(N) avg | SC: O(K)
```

---

## 48. Number of Distinct Substrings (Hashing Approach)

**Optimal uses Suffix Array / Suffix Automaton in O(N log N) or O(N). Hashing version:**

```cpp
int distinctSubstrings(string s) {
    unordered_set<string> seen;
    int n = s.size();
    for (int i = 0; i < n; i++) {
        for (int len = 1; len <= n - i; len++) {
            seen.insert(s.substr(i, len));
        }
    }
    return (int)seen.size();
}
// TC: O(N³) avg | SC: O(N³) — feasible for small N
// For large N, use rolling hash + set<long long>, or Suffix Automaton
```

---

## 49. Longest Duplicate Substring (Rabin-Karp + Binary Search)

```cpp
class Solution {
    const long long MOD = 1e18;
    const long long BASE = 26;

    int searchDup(string& s, int len) {
        long long h = 0, power = 1;
        for (int i = 0; i < len; i++) {
            h = (h * BASE + s[i] - 'a') % MOD;
            if (i < len - 1) power = (power * BASE) % MOD;
        }
        unordered_map<long long, vector<int>> seen;
        seen[h].push_back(0);
        for (int i = 1; i + len <= s.size(); i++) {
            h = (h - (s[i - 1] - 'a') * power % MOD + MOD) % MOD;
            h = (h * BASE + s[i + len - 1] - 'a') % MOD;
            if (seen.count(h)) {
                for (int idx : seen[h])
                    if (s.compare(idx, len, s, i, len) == 0) return i;
            }
            seen[h].push_back(i);
        }
        return -1;
    }

public:
    string longestDupSubstring(string s) {
        int low = 1, high = s.size() - 1, start = 0, maxLen = 0;
        while (low <= high) {
            int mid = (low + high) / 2;
            int idx = searchDup(s, mid);
            if (idx != -1) { start = idx; maxLen = mid; low = mid + 1; }
            else high = mid - 1;
        }
        return s.substr(start, maxLen);
    }
};
// TC: O(N log N) avg | SC: O(N)
```

---

## 50. Two Sum III — Data Structure Design

```cpp
class TwoSum {
    unordered_map<int, int> count;
public:
    void add(int x) { count[x]++; }

    bool find(int target) {
        for (auto& [num, c] : count) {
            int need = target - num;
            if (num == need && c >= 2) return true;
            if (num != need && count.count(need)) return true;
        }
        return false;
    }
};
// TC: add O(1), find O(N) | SC: O(N)
```

---

## 51. All O(1) Data Structure — LeetCode 432

**Insert / Delete / GetMaxKey / GetMinKey all in O(1).** Uses doubly linked list of buckets, each holding keys at the same count.

```cpp
class AllOne {
    struct Bucket { int count; unordered_set<string> keys; };
    list<Bucket> buckets;
    unordered_map<string, list<Bucket>::iterator> keyIt;
public:
    void inc(string key) {
        if (!keyIt.count(key)) {
            if (buckets.empty() || buckets.front().count != 1)
                buckets.push_front({1, {}});
            buckets.front().keys.insert(key);
            keyIt[key] = buckets.begin();
        } else {
            auto it = keyIt[key], next = std::next(it);
            if (next == buckets.end() || next->count != it->count + 1)
                next = buckets.insert(next, {it->count + 1, {}});
            next->keys.insert(key);
            keyIt[key] = next;
            it->keys.erase(key);
            if (it->keys.empty()) buckets.erase(it);
        }
    }

    void dec(string key) {
        auto it = keyIt[key];
        if (it->count == 1) {
            it->keys.erase(key);
            if (it->keys.empty()) buckets.erase(it);
            keyIt.erase(key);
            return;
        }
        auto prev = (it == buckets.begin()) ? buckets.end() : std::prev(it);
        if (prev == buckets.end() || prev->count != it->count - 1)
            prev = buckets.insert(it, {it->count - 1, {}});
        prev->keys.insert(key);
        keyIt[key] = prev;
        it->keys.erase(key);
        if (it->keys.empty()) buckets.erase(it);
    }

    string getMaxKey() { return buckets.empty() ? "" : *buckets.back().keys.begin(); }
    string getMinKey() { return buckets.empty() ? "" : *buckets.front().keys.begin(); }
};
// TC: O(1) per op | SC: O(N)
```

---

## 52. Encode and Decode TinyURL

```cpp
class Codec {
    unordered_map<string, string> codeToUrl;
    unordered_map<string, string> urlToCode;
    const string base = "http://tiny.url/";
    const string chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    string randomCode() {
        string code;
        for (int i = 0; i < 6; i++) code += chars[rand() % chars.size()];
        return code;
    }

public:
    string encode(string longUrl) {
        if (urlToCode.count(longUrl)) return base + urlToCode[longUrl];
        string code;
        do { code = randomCode(); } while (codeToUrl.count(code));
        codeToUrl[code] = longUrl;
        urlToCode[longUrl] = code;
        return base + code;
    }

    string decode(string shortUrl) {
        return codeToUrl[shortUrl.substr(base.size())];
    }
};
// TC: O(1) avg per op | SC: O(N)
```

---

## 53. Time-Based Key-Value Store

```cpp
class TimeMap {
    unordered_map<string, vector<pair<int, string>>> store;  // key -> [(time, value)]
public:
    void set(string key, string value, int timestamp) {
        store[key].push_back({timestamp, value});
    }

    string get(string key, int timestamp) {
        if (!store.count(key)) return "";
        auto& v = store[key];
        // Binary search for largest time <= timestamp
        int lo = 0, hi = v.size() - 1, ans = -1;
        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (v[mid].first <= timestamp) { ans = mid; lo = mid + 1; }
            else hi = mid - 1;
        }
        return ans == -1 ? "" : v[ans].second;
    }
};
// TC: set O(1), get O(log N) | SC: O(N)
```

---

## 54. Snapshot Array

```cpp
class SnapshotArray {
    vector<map<int, int>> data;  // index -> {snapId -> value}
    int snapId = 0;
public:
    SnapshotArray(int length) : data(length) {
        for (auto& m : data) m[0] = 0;
    }

    void set(int index, int val) { data[index][snapId] = val; }

    int snap() { return snapId++; }

    int get(int index, int snap_id) {
        auto it = data[index].upper_bound(snap_id);
        --it;
        return it->second;
    }
};
// TC: set O(log N), snap O(1), get O(log N) | SC: O(total writes)
```

---

## 55. Design HashMap from Scratch

**Separate chaining (each bucket is a list).**

```cpp
class MyHashMap {
    static const int SIZE = 1009;  // prime
    vector<list<pair<int, int>>> table;

    int hash(int key) { return key % SIZE; }

public:
    MyHashMap() : table(SIZE) {}

    void put(int key, int value) {
        auto& chain = table[hash(key)];
        for (auto& p : chain) {
            if (p.first == key) { p.second = value; return; }
        }
        chain.push_back({key, value});
    }

    int get(int key) {
        auto& chain = table[hash(key)];
        for (auto& p : chain) if (p.first == key) return p.second;
        return -1;
    }

    void remove(int key) {
        auto& chain = table[hash(key)];
        chain.remove_if([key](auto& p){ return p.first == key; });
    }
};
// TC: O(1) avg per op | SC: O(N)
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Pattern Recognition

| Problem Signal | Technique |
|----------------|-----------|
| "Count / frequency of elements" | `unordered_map<T, int>` |
| "Has duplicate / unique?" | `unordered_set` |
| "Subarray sum = K" | Prefix sum + hashmap |
| "Longest subarray with property" | Hashmap (first occurrence) + prefix |
| "Count subarrays with property" | Hashmap (count of occurrences) + prefix |
| "Need ordered keys / floor / ceil" | `map` / `set` (use `lower_bound` / `upper_bound`) |
| "Top K / Frequency sort" | Hashmap + heap or bucket sort |
| "Anagram / permutation match" | Frequency array (size 26 / 256) |
| "Cache / LRU / LFU" | Hashmap + doubly linked list |
| "Insert / Delete / GetRandom O(1)" | Hashmap + dynamic array |
| "Equal 0/1, divisible-by-K" | Replace value + prefix sum + hashmap |
| "Sliding window with constraints" | Hashmap of window contents |

---

## ⭐ Top 10 Must-Know

1. **Two Sum** — Hashmap of complements
2. **Frequency Map** — `unordered_map<T, int>`
3. **Prefix Sum + Hashmap** — count / longest subarray with sum K
4. **Sliding Window + Hashmap** — longest substring no repeat / min window
5. **Set for Distinct / Existence** — `unordered_set::count`
6. **Group by Key** — `unordered_map<key, vector<...>>` (anagrams, shifted strings)
7. **Top K Frequent** — bucket sort or heap
8. **LRU Cache** — list + iterator hashmap
9. **Insert/Delete/GetRandom O(1)** — vector + index hashmap
10. **0/1 → −1/+1 Trick** — converts equal-count problems to sum-0 problems

---

## ⭐ Common Pitfalls

✅ **`unordered_map[k]` creates entry** if `k` doesn't exist (default-constructed value). Use `count(k)` or `find(k)` to test existence without inserting.
✅ **Iterator invalidation** — modifying `unordered_map` while iterating may rehash. Save keys to delete first, then erase.
✅ **`map::erase(it)`** returns next iterator — useful for safe deletion in loops.
✅ **Negative modulo** — for `sum % k`, use `((sum % k) + k) % k` to get a non-negative remainder.
✅ **`prefix[0] = 1` (or `-1` for indices)** — sentinel for "subarray starts from index 0" cases.
✅ **`multiset::erase(value)`** removes ALL occurrences. Use `ms.erase(ms.find(value))` for one.
✅ **Hash collisions** — anti-test cases can degrade `unordered_map` to O(N). For competitive, use custom hash:

```cpp
struct CustomHash {
    size_t operator()(long long x) const {
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9LL;
        x = (x ^ (x >> 27)) * 0x94d049bb133111ebLL;
        return x ^ (x >> 31);
    }
};
unordered_map<long long, int, CustomHash> safeMap;
```

✅ **Pair / tuple as map key** — `unordered_map` requires custom hash. `map<pair<...>, V>` works out of the box.
✅ **String as key** — both `map` and `unordered_map` support it natively.
✅ **`structured bindings`** for cleaner iteration: `for (auto& [k, v] : mp)`.

---

## ⭐ Useful STL Snippets

```cpp
// Existence check (no accidental insertion)
if (mp.find(key) != mp.end()) { ... }
if (mp.count(key)) { ... }              // same effect, simpler

// Update or default
mp[key]++;                               // increments, creates with 0 first
mp[key] = mp.count(key) ? mp[key] + 1 : 1;

// Erase while iterating (map)
for (auto it = mp.begin(); it != mp.end(); ) {
    if (cond(it->first)) it = mp.erase(it);
    else ++it;
}

// Find next/prev key (ordered map)
auto it = mp.lower_bound(x);             // first key >= x
auto it = mp.upper_bound(x);             // first key >  x

// Reserve capacity to avoid rehashing
unordered_map<int, int> mp;
mp.reserve(N);
mp.max_load_factor(0.25);                // tighter, faster

// Convert vector → set → vector (dedup + sort)
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

// Get keys / values from map
vector<int> keys;
for (auto& [k, v] : mp) keys.push_back(k);
```

---

## ⭐ Tricks Worth Remembering

| Trick | Use Case |
|-------|----------|
| `prefixCount[0] = 1` | Subarray sum K count (handles "starts at 0") |
| `firstIdx[0] = -1` | Longest subarray with sum K |
| `0 → -1, 1 → +1` | Equal 0s and 1s problem |
| `(sum % k + k) % k` | Sum-divisible-by-k bucketing |
| `XOR ^ k` lookup | Subarray XOR = k |
| `exactly(K) = atMost(K) - atMost(K-1)` | "exactly K distinct" sliding window |
| Hashmap + Doubly Linked List | LRU cache (O(1) per op) |
| Hashmap + dynamic array | Insert/Delete/Random O(1) |
| Hashmap of `freq → list of keys` | LFU cache, AllOne |
| Sorted-string key | Group anagrams |
| Difference-from-first key | Group shifted strings |

---

# 💪 GO ACE THAT TEST!

> **Strategy on the test:**
> 1. **Spot the signal:** "count" / "first occurrence" / "duplicate" → almost always hashing.
> 2. **Choose the right structure:** ordered (`map`) only if you need `lower_bound` / smallest / largest.
> 3. **Initialize sentinels:** `prefixCount[0] = 1` for counts, `firstIdx[0] = -1` for indices.
> 4. **Watch existence vs insertion:** prefer `count(k)` over `mp[k]`.
> 5. **Use frequency arrays for fixed alphabets** (26, 256) — faster than hashmap.
> 6. **State complexity:** O(1) average for hashing, O(log N) for tree-based.

**You've got this! 🚀**
