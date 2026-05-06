# 🔤 Complete String Problems Interview Sheet — C++

> **The ultimate string problems guide for coding interviews & tests.**
> Reverse, Palindrome, Anagram, Pattern Matching, Sliding Window, KMP, Z-Algo & more.

---

## 📑 Table of Contents

**BASIC (1–18)**
1. Reverse a String
2. Reverse Words in a String
3. Check Palindrome String
4. Check Two Strings are Anagrams
5. Count Vowels and Consonants
6. Convert String to Lowercase / Uppercase
7. Count Occurrences of a Character
8. Remove All Occurrences of a Character
9. Remove Duplicate Characters
10. Check if String Contains Only Digits
11. Toggle Case of Each Character
12. Check Two Strings are Rotations
13. Find Length Without strlen
14. Concatenate Two Strings
15. Compare Two Strings
16. Largest Word in a String
17. Count Words in a String
18. Print All Substrings

**INTERMEDIATE (19–40)**
19. Check Valid Palindrome (Alphanumeric only)
20. Longest Common Prefix
21. Valid Parentheses (Balanced Brackets)
22. Implement strStr() / Find Substring
23. Roman to Integer & Integer to Roman
24. String to Integer (atoi)
25. Add Two Strings (Big Numbers)
26. Multiply Two Strings
27. Longest Palindromic Substring
28. Count Palindromic Substrings
29. Group Anagrams
30. Longest Substring Without Repeating Characters
31. Longest Repeating Character Replacement
32. Minimum Window Substring
33. Permutations of a String
34. All Subsequences of a String
35. Check If One String is Subsequence of Another
36. Reverse Vowels of a String
37. Compress String (Run-Length Encoding)
38. First Non-Repeating Character
39. Most Frequent Character
40. Sort Characters by Frequency

**ADVANCED (41–55)**
41. KMP Algorithm — Pattern Matching
42. Rabin-Karp Algorithm — Rolling Hash
43. Z-Algorithm
44. Manacher's Algorithm — Longest Palindrome O(N)
45. Edit Distance / Levenshtein Distance
46. Longest Common Subsequence
47. Longest Common Substring
48. Longest Palindromic Subsequence
49. Wildcard Pattern Matching
50. Regular Expression Matching
51. Word Break Problem
52. Minimum Insertions to Make Palindrome
53. Distinct Subsequences
54. Shortest Palindrome
55. Repeated Substring Pattern

---

## ⚡ Quick Complexity Reference

| Operation | Time | Notes |
|-----------|------|-------|
| Reverse / palindrome check | O(N) | Two pointers |
| Anagram check | O(N) | Frequency count or sort |
| Substring search (naive) | O(N×M) | Brute force |
| KMP / Z-algo | O(N+M) | Linear pattern match |
| Rabin-Karp | O(N+M) avg | Rolling hash |
| All substrings | O(N²) substrings, O(N³) print | Hashing reduces |
| Longest palindrome (DP) | O(N²) time, O(N²) space | Or expand-around-center |
| Manacher's | O(N) | Optimal palindromes |
| Edit distance / LCS | O(N×M) | Classic DP |
| Permutations | O(N! × N) | Backtracking |

---

# 🟢 BASIC PROBLEMS

---

## 1. Reverse a String

```cpp
#include <bits/stdc++.h>
using namespace std;

// Two-pointer in-place
void reverseString(string& s) {
    int l = 0, r = s.size() - 1;
    while (l < r) swap(s[l++], s[r--]);
}

// Recursive
void reverseRec(string& s, int l, int r) {
    if (l >= r) return;
    swap(s[l], s[r]);
    reverseRec(s, l + 1, r - 1);
}
// TC: O(N) | SC: O(1) iterative, O(N) recursive
```

---

## 2. Reverse Words in a String

**Input:** `"the sky is blue"` → **Output:** `"blue is sky the"`

```cpp
string reverseWords(string s) {
    // Step 1: reverse the whole string
    reverse(s.begin(), s.end());
    int n = s.size(), idx = 0;
    for (int i = 0; i < n; i++) {
        if (s[i] != ' ') {
            if (idx != 0) s[idx++] = ' ';
            int j = i;
            // Step 2: copy each word
            while (j < n && s[j] != ' ') s[idx++] = s[j++];
            // Step 3: reverse each word
            reverse(s.begin() + idx - (j - i), s.begin() + idx);
            i = j;
        }
    }
    s.erase(s.begin() + idx, s.end());
    return s;
}
// TC: O(N) | SC: O(1) extra
```

---

## 3. Check Palindrome String

```cpp
bool isPalindrome(string s) {
    int l = 0, r = s.size() - 1;
    while (l < r) {
        if (s[l] != s[r]) return false;
        l++; r--;
    }
    return true;
}
// TC: O(N) | SC: O(1)
```

---

## 4. Check Two Strings are Anagrams

```cpp
bool isAnagram(string s, string t) {
    if (s.size() != t.size()) return false;
    int count[26] = {0};
    for (char c : s) count[c - 'a']++;
    for (char c : t) {
        if (--count[c - 'a'] < 0) return false;
    }
    return true;
}
// TC: O(N) | SC: O(1) — fixed 26
```

> For unicode/general chars: use `unordered_map<char, int>`.

---

## 5. Count Vowels and Consonants

```cpp
pair<int, int> countVowelsConsonants(string s) {
    int vowels = 0, consonants = 0;
    for (char c : s) {
        char ch = tolower(c);
        if (isalpha(ch)) {
            if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u')
                vowels++;
            else consonants++;
        }
    }
    return {vowels, consonants};
}
// TC: O(N) | SC: O(1)
```

---

## 6. Convert String to Lowercase / Uppercase

```cpp
string toLower(string s) {
    for (char& c : s) {
        if (c >= 'A' && c <= 'Z') c += 32;  // or c = tolower(c)
    }
    return s;
}

string toUpper(string s) {
    for (char& c : s) {
        if (c >= 'a' && c <= 'z') c -= 32;  // or c = toupper(c)
    }
    return s;
}
// TC: O(N) | SC: O(1) extra
```

---

## 7. Count Occurrences of a Character

```cpp
int countChar(string s, char ch) {
    int count = 0;
    for (char c : s) if (c == ch) count++;
    return count;
}
// Or: count(s.begin(), s.end(), ch);
// TC: O(N) | SC: O(1)
```

---

## 8. Remove All Occurrences of a Character

```cpp
string removeChar(string s, char ch) {
    int idx = 0;
    for (char c : s) if (c != ch) s[idx++] = c;
    s.resize(idx);
    return s;
}
// TC: O(N) | SC: O(1) in-place
```

---

## 9. Remove Duplicate Characters (preserve order)

```cpp
string removeDuplicates(string s) {
    bool seen[256] = {false};
    string res;
    for (char c : s) {
        if (!seen[(unsigned char)c]) {
            seen[(unsigned char)c] = true;
            res += c;
        }
    }
    return res;
}
// TC: O(N) | SC: O(1) — fixed alphabet
```

---

## 10. Check if String Contains Only Digits

```cpp
bool isAllDigits(string s) {
    if (s.empty()) return false;
    for (char c : s) if (!isdigit(c)) return false;
    return true;
}
// TC: O(N) | SC: O(1)
```

---

## 11. Toggle Case of Each Character

```cpp
string toggleCase(string s) {
    for (char& c : s) {
        if (c >= 'a' && c <= 'z') c -= 32;
        else if (c >= 'A' && c <= 'Z') c += 32;
    }
    return s;
}
// XOR trick: c ^ 32 (works for letters)
// TC: O(N) | SC: O(1)
```

---

## 12. Check Two Strings are Rotations of Each Other

**Trick:** if `s2` is a rotation of `s1`, then `s2` is a substring of `s1 + s1`.

```cpp
bool areRotations(string s1, string s2) {
    if (s1.size() != s2.size()) return false;
    return (s1 + s1).find(s2) != string::npos;
}
// TC: O(N) using KMP-based find | SC: O(N)
```

---

## 13. Find Length Without strlen

```cpp
int strLength(const char* s) {
    int len = 0;
    while (s[len] != '\0') len++;
    return len;
}
// For std::string: simply use s.size() / s.length()
// TC: O(N) | SC: O(1)
```

---

## 14. Concatenate Two Strings (Manual)

```cpp
string concatenate(string a, string b) {
    string res;
    res.reserve(a.size() + b.size());
    for (char c : a) res += c;
    for (char c : b) res += c;
    return res;
}
// Or simply: return a + b;
// TC: O(N + M) | SC: O(N + M)
```

---

## 15. Compare Two Strings

```cpp
int compareStrings(string a, string b) {
    int n = min(a.size(), b.size());
    for (int i = 0; i < n; i++) {
        if (a[i] != b[i]) return a[i] - b[i];
    }
    return (int)a.size() - (int)b.size();
}
// Returns: 0 (equal), <0 (a < b), >0 (a > b)
// Or: a.compare(b);
// TC: O(N) | SC: O(1)
```

---

## 16. Largest Word in a String

```cpp
string largestWord(string s) {
    string longest, current;
    s += ' ';  // sentinel
    for (char c : s) {
        if (c == ' ') {
            if (current.size() > longest.size()) longest = current;
            current.clear();
        } else current += c;
    }
    return longest;
}
// TC: O(N) | SC: O(N)
```

---

## 17. Count Words in a String

```cpp
int countWords(string s) {
    int count = 0;
    int n = s.size(), i = 0;
    while (i < n) {
        while (i < n && s[i] == ' ') i++;
        if (i < n) {
            count++;
            while (i < n && s[i] != ' ') i++;
        }
    }
    return count;
}
// TC: O(N) | SC: O(1)
```

---

## 18. Print All Substrings

```cpp
vector<string> allSubstrings(string s) {
    vector<string> result;
    int n = s.size();
    for (int i = 0; i < n; i++) {
        for (int len = 1; len <= n - i; len++) {
            result.push_back(s.substr(i, len));
        }
    }
    return result;
}
// TC: O(N³) — N² substrings × O(N) substr | SC: O(N³)
// Total substrings = N*(N+1)/2
```

---

# 🟡 INTERMEDIATE PROBLEMS

---

## 19. Valid Palindrome — Alphanumeric Only

**Input:** `"A man, a plan, a canal: Panama"` → **true**

```cpp
bool isValidPalindrome(string s) {
    int l = 0, r = s.size() - 1;
    while (l < r) {
        while (l < r && !isalnum(s[l])) l++;
        while (l < r && !isalnum(s[r])) r--;
        if (tolower(s[l]) != tolower(s[r])) return false;
        l++; r--;
    }
    return true;
}
// TC: O(N) | SC: O(1)
```

---

## 20. Longest Common Prefix

```cpp
string longestCommonPrefix(vector<string>& strs) {
    if (strs.empty()) return "";
    sort(strs.begin(), strs.end());
    string a = strs.front(), b = strs.back();
    int i = 0;
    while (i < a.size() && i < b.size() && a[i] == b[i]) i++;
    return a.substr(0, i);
}
// TC: O(N log N × M) | SC: O(1)

// Vertical scan alternative — O(N × M)
string lcpVertical(vector<string>& strs) {
    if (strs.empty()) return "";
    for (int i = 0; i < strs[0].size(); i++) {
        char c = strs[0][i];
        for (int j = 1; j < strs.size(); j++) {
            if (i >= strs[j].size() || strs[j][i] != c)
                return strs[0].substr(0, i);
        }
    }
    return strs[0];
}
```

---

## 21. Valid Parentheses (Balanced Brackets)

```cpp
bool isValidParens(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') st.push(c);
        else {
            if (st.empty()) return false;
            char top = st.top();
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) return false;
            st.pop();
        }
    }
    return st.empty();
}
// TC: O(N) | SC: O(N)
```

---

## 22. Implement strStr() / Find Substring (Naive + KMP teaser)

```cpp
// Naive — O(N×M)
int strStr(string haystack, string needle) {
    int n = haystack.size(), m = needle.size();
    if (m == 0) return 0;
    for (int i = 0; i <= n - m; i++) {
        int j = 0;
        while (j < m && haystack[i + j] == needle[j]) j++;
        if (j == m) return i;
    }
    return -1;
}
// For optimal O(N+M), see KMP (#41) or Z-algo (#43)
```

---

## 23. Roman ↔ Integer

```cpp
int romanToInt(string s) {
    unordered_map<char, int> mp = {
        {'I',1},{'V',5},{'X',10},{'L',50},
        {'C',100},{'D',500},{'M',1000}
    };
    int sum = 0, n = s.size();
    for (int i = 0; i < n; i++) {
        if (i + 1 < n && mp[s[i]] < mp[s[i+1]]) sum -= mp[s[i]];
        else sum += mp[s[i]];
    }
    return sum;
}

string intToRoman(int num) {
    vector<pair<int, string>> v = {
        {1000,"M"},{900,"CM"},{500,"D"},{400,"CD"},
        {100,"C"},{90,"XC"},{50,"L"},{40,"XL"},
        {10,"X"},{9,"IX"},{5,"V"},{4,"IV"},{1,"I"}
    };
    string res;
    for (auto& [val, sym] : v) {
        while (num >= val) { res += sym; num -= val; }
    }
    return res;
}
// TC: O(N) | SC: O(1)
```

---

## 24. String to Integer (atoi)

```cpp
int myAtoi(string s) {
    int i = 0, n = s.size(), sign = 1;
    long long result = 0;
    while (i < n && s[i] == ' ') i++;
    if (i < n && (s[i] == '+' || s[i] == '-')) {
        sign = (s[i] == '-') ? -1 : 1;
        i++;
    }
    while (i < n && isdigit(s[i])) {
        result = result * 10 + (s[i] - '0');
        if (sign * result > INT_MAX) return INT_MAX;
        if (sign * result < INT_MIN) return INT_MIN;
        i++;
    }
    return (int)(sign * result);
}
// TC: O(N) | SC: O(1)
```

---

## 25. Add Two Strings (Big Numbers)

```cpp
string addStrings(string a, string b) {
    string res;
    int i = a.size() - 1, j = b.size() - 1, carry = 0;
    while (i >= 0 || j >= 0 || carry) {
        int sum = carry;
        if (i >= 0) sum += a[i--] - '0';
        if (j >= 0) sum += b[j--] - '0';
        res += char('0' + sum % 10);
        carry = sum / 10;
    }
    reverse(res.begin(), res.end());
    return res;
}
// TC: O(max(N, M)) | SC: O(max(N, M))
```

---

## 26. Multiply Two Strings

```cpp
string multiplyStrings(string a, string b) {
    if (a == "0" || b == "0") return "0";
    int n = a.size(), m = b.size();
    vector<int> result(n + m, 0);
    for (int i = n - 1; i >= 0; i--) {
        for (int j = m - 1; j >= 0; j--) {
            int mul = (a[i] - '0') * (b[j] - '0');
            int p1 = i + j, p2 = i + j + 1;
            int sum = mul + result[p2];
            result[p2] = sum % 10;
            result[p1] += sum / 10;
        }
    }
    string res;
    for (int x : result) if (!(res.empty() && x == 0)) res += char('0' + x);
    return res.empty() ? "0" : res;
}
// TC: O(N×M) | SC: O(N+M)
```

---

## 27. Longest Palindromic Substring

**Expand-around-center — O(N²) time, O(1) space:**

```cpp
string longestPalindrome(string s) {
    if (s.empty()) return "";
    int start = 0, maxLen = 1, n = s.size();

    auto expand = [&](int l, int r) {
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxLen) {
                maxLen = r - l + 1;
                start = l;
            }
            l--; r++;
        }
    };

    for (int i = 0; i < n; i++) {
        expand(i, i);       // odd length
        expand(i, i + 1);   // even length
    }
    return s.substr(start, maxLen);
}
// TC: O(N²) | SC: O(1)
// For O(N), see Manacher's #44
```

---

## 28. Count Palindromic Substrings

```cpp
int countSubstrings(string s) {
    int count = 0, n = s.size();

    auto expand = [&](int l, int r) {
        while (l >= 0 && r < n && s[l] == s[r]) {
            count++;
            l--; r++;
        }
    };

    for (int i = 0; i < n; i++) {
        expand(i, i);
        expand(i, i + 1);
    }
    return count;
}
// TC: O(N²) | SC: O(1)
```

---

## 29. Group Anagrams

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
// TC: O(N × K log K) where K = max string length | SC: O(N × K)

// Alternative: count-based key (O(N × K))
string getKey(string s) {
    int count[26] = {0};
    for (char c : s) count[c - 'a']++;
    string key;
    for (int x : count) key += to_string(x) + "#";
    return key;
}
```

---

## 30. Longest Substring Without Repeating Characters

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
// TC: O(N) | SC: O(min(N, alphabet))
```

---

## 31. Longest Repeating Character Replacement

**Replace at most K characters; find longest substring of same letter.**

```cpp
int characterReplacement(string s, int k) {
    int count[26] = {0};
    int left = 0, maxFreq = 0, maxLen = 0;
    for (int right = 0; right < s.size(); right++) {
        count[s[right] - 'A']++;
        maxFreq = max(maxFreq, count[s[right] - 'A']);
        // Window invalid: shrink
        while ((right - left + 1) - maxFreq > k) {
            count[s[left] - 'A']--;
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
// TC: O(N) | SC: O(1)
```

---

## 32. Minimum Window Substring

**Find smallest substring of `s` containing all characters of `t`.**

```cpp
string minWindow(string s, string t) {
    if (t.empty() || s.size() < t.size()) return "";
    int need[128] = {0};
    for (char c : t) need[(int)c]++;
    int required = t.size();
    int left = 0, minLen = INT_MAX, minStart = 0;
    for (int right = 0; right < s.size(); right++) {
        if (need[(int)s[right]]-- > 0) required--;
        while (required == 0) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minStart = left;
            }
            if (++need[(int)s[left]] > 0) required++;
            left++;
        }
    }
    return minLen == INT_MAX ? "" : s.substr(minStart, minLen);
}
// TC: O(N) | SC: O(1) — fixed alphabet
```

---

## 33. Permutations of a String

```cpp
void permuteHelper(string& s, int idx, vector<string>& result) {
    if (idx == s.size()) {
        result.push_back(s);
        return;
    }
    for (int i = idx; i < s.size(); i++) {
        swap(s[idx], s[i]);
        permuteHelper(s, idx + 1, result);
        swap(s[idx], s[i]);  // backtrack
    }
}

vector<string> permutations(string s) {
    vector<string> result;
    permuteHelper(s, 0, result);
    return result;
}
// TC: O(N! × N) | SC: O(N) recursion

// Unique permutations — sort and skip duplicates
// Or use std::next_permutation in a loop:
vector<string> uniquePermutations(string s) {
    sort(s.begin(), s.end());
    vector<string> result;
    do { result.push_back(s); }
    while (next_permutation(s.begin(), s.end()));
    return result;
}
```

---

## 34. All Subsequences of a String

**A subsequence keeps order but may skip characters. 2^N subsequences total.**

```cpp
void subseqHelper(string& s, int idx, string current, vector<string>& result) {
    if (idx == s.size()) {
        result.push_back(current);
        return;
    }
    // Exclude
    subseqHelper(s, idx + 1, current, result);
    // Include
    subseqHelper(s, idx + 1, current + s[idx], result);
}

vector<string> subsequences(string s) {
    vector<string> result;
    subseqHelper(s, 0, "", result);
    return result;
}
// TC: O(2^N × N) | SC: O(N) recursion

// Iterative bitmask
vector<string> subseqBitmask(string s) {
    int n = s.size();
    vector<string> result;
    for (int mask = 0; mask < (1 << n); mask++) {
        string current;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) current += s[i];
        }
        result.push_back(current);
    }
    return result;
}
```

---

## 35. Check If One String is Subsequence of Another

```cpp
bool isSubsequence(string s, string t) {
    int i = 0, j = 0;
    while (i < s.size() && j < t.size()) {
        if (s[i] == t[j]) i++;
        j++;
    }
    return i == s.size();
}
// TC: O(N + M) | SC: O(1)
```

---

## 36. Reverse Vowels of a String

```cpp
string reverseVowels(string s) {
    auto isVowel = [](char c) {
        c = tolower(c);
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    };
    int l = 0, r = s.size() - 1;
    while (l < r) {
        while (l < r && !isVowel(s[l])) l++;
        while (l < r && !isVowel(s[r])) r--;
        swap(s[l++], s[r--]);
    }
    return s;
}
// TC: O(N) | SC: O(1)
```

---

## 37. Compress String — Run-Length Encoding

**Input:** `"aaabbc"` → **Output:** `"a3b2c1"`

```cpp
string compressString(string s) {
    if (s.empty()) return "";
    string res;
    int n = s.size();
    for (int i = 0; i < n;) {
        int j = i;
        while (j < n && s[j] == s[i]) j++;
        res += s[i] + to_string(j - i);
        i = j;
    }
    return res;
}
// TC: O(N) | SC: O(N)

// LeetCode 443 variant — only add count if > 1, in-place
int compressInPlace(vector<char>& chars) {
    int n = chars.size(), idx = 0, i = 0;
    while (i < n) {
        int j = i;
        while (j < n && chars[j] == chars[i]) j++;
        chars[idx++] = chars[i];
        if (j - i > 1) {
            for (char c : to_string(j - i)) chars[idx++] = c;
        }
        i = j;
    }
    return idx;
}
```

---

## 38. First Non-Repeating Character

```cpp
int firstUniqChar(string s) {
    int count[26] = {0};
    for (char c : s) count[c - 'a']++;
    for (int i = 0; i < s.size(); i++) {
        if (count[s[i] - 'a'] == 1) return i;
    }
    return -1;
}
// TC: O(N) | SC: O(1)
```

---

## 39. Most Frequent Character

```cpp
char mostFrequent(string s) {
    int count[256] = {0};
    int maxCount = 0;
    char res = s[0];
    for (char c : s) {
        count[(unsigned char)c]++;
        if (count[(unsigned char)c] > maxCount) {
            maxCount = count[(unsigned char)c];
            res = c;
        }
    }
    return res;
}
// TC: O(N) | SC: O(1)
```

---

## 40. Sort Characters by Frequency

```cpp
string frequencySort(string s) {
    unordered_map<char, int> freq;
    for (char c : s) freq[c]++;
    vector<pair<char, int>> v(freq.begin(), freq.end());
    sort(v.begin(), v.end(), [](auto& a, auto& b) {
        return a.second > b.second;
    });
    string result;
    for (auto& [c, f] : v) result += string(f, c);
    return result;
}
// TC: O(N + K log K) | SC: O(N)

// Bucket-sort approach — O(N)
string frequencySortBucket(string s) {
    unordered_map<char, int> freq;
    for (char c : s) freq[c]++;
    vector<string> buckets(s.size() + 1);
    for (auto& [c, f] : freq) buckets[f] += string(f, c);
    string res;
    for (int i = buckets.size() - 1; i >= 0; i--) res += buckets[i];
    return res;
}
```

---

# 🔴 ADVANCED PROBLEMS

---

## 41. KMP Algorithm — Pattern Matching

**Find all occurrences of pattern in text in O(N+M).**

```cpp
vector<int> computeLPS(string& pattern) {
    int m = pattern.size();
    vector<int> lps(m, 0);
    int len = 0, i = 1;
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            lps[i++] = ++len;
        } else if (len != 0) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return lps;
}

vector<int> KMPSearch(string text, string pattern) {
    vector<int> result;
    int n = text.size(), m = pattern.size();
    if (m == 0) return result;
    vector<int> lps = computeLPS(pattern);
    int i = 0, j = 0;
    while (i < n) {
        if (text[i] == pattern[j]) { i++; j++; }
        if (j == m) {
            result.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && text[i] != pattern[j]) {
            if (j != 0) j = lps[j - 1];
            else i++;
        }
    }
    return result;
}
// TC: O(N + M) | SC: O(M)
```

---

## 42. Rabin-Karp Algorithm — Rolling Hash

```cpp
vector<int> rabinKarp(string text, string pattern) {
    vector<int> result;
    int n = text.size(), m = pattern.size();
    if (m > n || m == 0) return result;
    const int BASE = 256;
    const int MOD = 1e9 + 7;
    long long patHash = 0, textHash = 0, h = 1;
    for (int i = 0; i < m - 1; i++) h = (h * BASE) % MOD;
    for (int i = 0; i < m; i++) {
        patHash = (BASE * patHash + pattern[i]) % MOD;
        textHash = (BASE * textHash + text[i]) % MOD;
    }
    for (int i = 0; i <= n - m; i++) {
        if (patHash == textHash) {
            // Verify (avoid collisions)
            if (text.compare(i, m, pattern) == 0) result.push_back(i);
        }
        if (i < n - m) {
            textHash = (BASE * (textHash - text[i] * h) + text[i + m]) % MOD;
            if (textHash < 0) textHash += MOD;
        }
    }
    return result;
}
// TC: O(N + M) average, O(N×M) worst | SC: O(1)
```

---

## 43. Z-Algorithm

**Z[i] = length of longest substring starting at i that matches a prefix.**

```cpp
vector<int> zFunction(string s) {
    int n = s.size();
    vector<int> z(n, 0);
    int l = 0, r = 0;
    for (int i = 1; i < n; i++) {
        if (i < r) z[i] = min(r - i, z[i - l]);
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) z[i]++;
        if (i + z[i] > r) {
            l = i;
            r = i + z[i];
        }
    }
    return z;
}

// Pattern search using Z-algo
vector<int> zSearch(string text, string pattern) {
    string concat = pattern + "$" + text;
    vector<int> z = zFunction(concat);
    vector<int> result;
    int m = pattern.size();
    for (int i = m + 1; i < concat.size(); i++) {
        if (z[i] == m) result.push_back(i - m - 1);
    }
    return result;
}
// TC: O(N + M) | SC: O(N + M)
```

---

## 44. Manacher's Algorithm — Longest Palindromic Substring O(N)

```cpp
string longestPalindromeManacher(string s) {
    if (s.empty()) return "";
    // Transform: "abc" → "^#a#b#c#$"
    string t = "^";
    for (char c : s) { t += '#'; t += c; }
    t += "#$";
    int n = t.size();
    vector<int> p(n, 0);
    int center = 0, right = 0;
    for (int i = 1; i < n - 1; i++) {
        int mirror = 2 * center - i;
        if (i < right) p[i] = min(right - i, p[mirror]);
        while (t[i + p[i] + 1] == t[i - p[i] - 1]) p[i]++;
        if (i + p[i] > right) {
            center = i;
            right = i + p[i];
        }
    }
    int maxLen = 0, centerIdx = 0;
    for (int i = 1; i < n - 1; i++) {
        if (p[i] > maxLen) {
            maxLen = p[i];
            centerIdx = i;
        }
    }
    int start = (centerIdx - maxLen) / 2;
    return s.substr(start, maxLen);
}
// TC: O(N) | SC: O(N)
```

---

## 45. Edit Distance / Levenshtein Distance

**Min insertions/deletions/replacements to transform `s1` → `s2`.**

```cpp
int editDistance(string s1, string s2) {
    int n = s1.size(), m = s2.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = 0; i <= n; i++) dp[i][0] = i;
    for (int j = 0; j <= m; j++) dp[0][j] = j;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (s1[i-1] == s2[j-1]) dp[i][j] = dp[i-1][j-1];
            else dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
        }
    }
    return dp[n][m];
}
// TC: O(N×M) | SC: O(N×M); reducible to O(min(N,M))
```

---

## 46. Longest Common Subsequence (LCS)

```cpp
int LCS(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a[i-1] == b[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
            else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[n][m];
}
// TC: O(N×M) | SC: O(N×M)

// Print the LCS
string printLCS(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            dp[i][j] = (a[i-1] == b[j-1]) ? dp[i-1][j-1] + 1
                                          : max(dp[i-1][j], dp[i][j-1]);
    string lcs;
    int i = n, j = m;
    while (i > 0 && j > 0) {
        if (a[i-1] == b[j-1]) { lcs += a[i-1]; i--; j--; }
        else if (dp[i-1][j] > dp[i][j-1]) i--;
        else j--;
    }
    reverse(lcs.begin(), lcs.end());
    return lcs;
}
```

---

## 47. Longest Common Substring

```cpp
int longestCommonSubstring(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    int maxLen = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a[i-1] == b[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
                maxLen = max(maxLen, dp[i][j]);
            }
            // No else — substring must be contiguous
        }
    }
    return maxLen;
}
// TC: O(N×M) | SC: O(N×M)
```

---

## 48. Longest Palindromic Subsequence

**Trick:** LPS(s) = LCS(s, reverse(s))

```cpp
int longestPalindromeSubseq(string s) {
    string r = s;
    reverse(r.begin(), r.end());
    return LCS(s, r);
}
// TC: O(N²) | SC: O(N²)
```

---

## 49. Wildcard Pattern Matching

**`?` matches any single char; `*` matches any sequence (incl. empty).**

```cpp
bool isMatchWildcard(string s, string p) {
    int n = s.size(), m = p.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(m + 1, false));
    dp[0][0] = true;
    for (int j = 1; j <= m; j++) {
        if (p[j-1] == '*') dp[0][j] = dp[0][j-1];
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (p[j-1] == '*') dp[i][j] = dp[i-1][j] || dp[i][j-1];
            else if (p[j-1] == '?' || p[j-1] == s[i-1]) dp[i][j] = dp[i-1][j-1];
        }
    }
    return dp[n][m];
}
// TC: O(N×M) | SC: O(N×M)
```

---

## 50. Regular Expression Matching

**`.` matches any char; `*` matches zero or more of preceding element.**

```cpp
bool isMatchRegex(string s, string p) {
    int n = s.size(), m = p.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(m + 1, false));
    dp[0][0] = true;
    for (int j = 2; j <= m; j++)
        if (p[j-1] == '*') dp[0][j] = dp[0][j-2];
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (p[j-1] == '.' || p[j-1] == s[i-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else if (p[j-1] == '*') {
                dp[i][j] = dp[i][j-2];  // zero occurrences
                if (p[j-2] == '.' || p[j-2] == s[i-1])
                    dp[i][j] = dp[i][j] || dp[i-1][j];
            }
        }
    }
    return dp[n][m];
}
// TC: O(N×M) | SC: O(N×M)
```

---

## 51. Word Break Problem

**Can `s` be segmented into space-separated words from a dictionary?**

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    int n = s.size();
    vector<bool> dp(n + 1, false);
    dp[0] = true;
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && dict.count(s.substr(j, i - j))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[n];
}
// TC: O(N² × L) where L = max word length | SC: O(N + dict)
```

---

## 52. Minimum Insertions to Make Palindrome

**Answer = N − LPS(s)**

```cpp
int minInsertionsToPalindrome(string s) {
    int n = s.size();
    return n - longestPalindromeSubseq(s);
}
// TC: O(N²) | SC: O(N²)
```

---

## 53. Distinct Subsequences

**Count subsequences of `s` that equal `t`.**

```cpp
int numDistinct(string s, string t) {
    int n = s.size(), m = t.size();
    vector<vector<unsigned long long>> dp(n + 1, vector<unsigned long long>(m + 1, 0));
    for (int i = 0; i <= n; i++) dp[i][0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (s[i-1] == t[j-1]) dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
            else dp[i][j] = dp[i-1][j];
        }
    }
    return (int)dp[n][m];
}
// TC: O(N×M) | SC: O(N×M)
```

---

## 54. Shortest Palindrome

**Add minimum chars to start of `s` to make it a palindrome.**

```cpp
string shortestPalindrome(string s) {
    string r = s;
    reverse(r.begin(), r.end());
    string combined = s + "#" + r;
    // KMP failure function on combined
    int n = combined.size();
    vector<int> lps(n, 0);
    for (int i = 1; i < n; i++) {
        int j = lps[i - 1];
        while (j > 0 && combined[i] != combined[j]) j = lps[j - 1];
        if (combined[i] == combined[j]) j++;
        lps[i] = j;
    }
    // lps[n-1] = longest palindromic prefix length
    return r.substr(0, s.size() - lps[n - 1]) + s;
}
// TC: O(N) | SC: O(N)
```

---

## 55. Repeated Substring Pattern

**Can `s` be constructed by repeating a substring multiple times?**

```cpp
// Trick: if s is repeated, then s appears in (s+s) excluding first/last chars.
bool repeatedSubstringPattern(string s) {
    string doubled = s + s;
    return doubled.substr(1, doubled.size() - 2).find(s) != string::npos;
}
// TC: O(N) using KMP-based find | SC: O(N)

// KMP-based length check
bool repeatedKMP(string s) {
    int n = s.size();
    vector<int> lps(n, 0);
    for (int i = 1; i < n; i++) {
        int j = lps[i - 1];
        while (j > 0 && s[i] != s[j]) j = lps[j - 1];
        if (s[i] == s[j]) j++;
        lps[i] = j;
    }
    int len = n - lps[n - 1];
    return lps[n - 1] > 0 && n % len == 0;
}
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Pattern Recognition

| Problem Signal | Technique |
|----------------|-----------|
| "Pattern in text" | KMP / Z-algorithm / Rabin-Karp |
| "Longest palindrome" | Expand-around-center / Manacher's |
| "Anagram / permutation" | Frequency count (26 / 256 array) |
| "Substring with property" | Sliding window |
| "Compare two strings" | DP — LCS / Edit distance |
| "Wildcard / regex" | DP with state for `*` |
| "All subsequences / permutations" | Backtracking / Bitmask |
| "Rotation check" | `(s+s).find(s2)` |
| "Repeated pattern" | KMP failure function |
| "Group by structure" | Hash with sorted/canonical key |

---

## ⭐ Top 10 Must-Know

1. **Two pointer** — palindrome, reverse, anagram
2. **Frequency array** — anagrams, char counts (size 26 or 256)
3. **Sliding window** — longest substring without repeats
4. **KMP** — linear pattern matching
5. **Z-algorithm** — pattern matching alternative
6. **Manacher's** — O(N) longest palindrome
7. **Rabin-Karp** — rolling hash
8. **DP on strings** — LCS, Edit Distance, Wildcard
9. **Backtracking** — permutations, word break
10. **Trie** (bonus) — prefix searches, autocomplete

---

## ⭐ Common Pitfalls

✅ **String vs C-string:** prefer `std::string` in C++.
✅ **`string::npos`** for "not found" — type is `size_t`.
✅ **`s.substr(start, len)`** — second arg is length, not end index.
✅ **`tolower / toupper`** require `unsigned char` cast for safety.
✅ **`isalnum / isalpha`** — handle locale; pass `unsigned char` value.
✅ **Modifying string while iterating** — use index or `erase` correctly.
✅ **Off-by-one** in DP table — sizes are N+1 × M+1.
✅ **Empty string edge case** — always test `s = ""`.
✅ **Index vs iterator:** `s.find(t)` returns position (size_t).
✅ **Negative subtraction:** `(int)a.size() - (int)b.size()` — cast to avoid wrap.

---

## ⭐ Useful STL Snippets

```cpp
// Searching
s.find("abc");                          // first occurrence, npos if not found
s.rfind("abc");                         // last occurrence
s.find_first_of("aeiou");               // first vowel
s.find_first_not_of(" \t\n");           // first non-whitespace

// Modifying
s.substr(start, len);                   // safe extract
s.insert(pos, "xyz");
s.erase(pos, len);
s.replace(pos, len, "new");
reverse(s.begin(), s.end());
sort(s.begin(), s.end());
transform(s.begin(), s.end(), s.begin(), ::tolower);

// Conversion
to_string(123);                         // int → string
stoi("123"); stol("123"); stoll("123");
stod("3.14");

// Splitting (using stringstream)
stringstream ss(s);
string token;
while (ss >> token) cout << token;       // splits by whitespace

// Char checks (cctype)
isalpha(c); isdigit(c); isalnum(c);
isspace(c); isupper(c); islower(c);
toupper(c); tolower(c);
```

---

## ⭐ Tricks Worth Remembering

| Trick | Use Case |
|-------|----------|
| `c ^ 32` | Toggle case (letters only) |
| `(s + s).find(t)` | Check rotation |
| `count[26]` instead of map | Lowercase-only strings |
| Sort string as anagram key | Group anagrams |
| `pattern + "$" + text` + Z | Single-pass pattern search |
| `s + "#" + reverse(s)` + KMP | Shortest palindrome |
| Reverse + LCS | Longest palindromic subsequence |
| Expand around center | Linear-space palindrome problems |

---

# 💪 GO ACE THAT TEST!

> **Strategy on the test:**
> 1. Identify if **alphabet is fixed** (26 lowercase) — use array, not map.
> 2. Spot **sliding window** signals: "longest", "shortest", "subarray/substring with…"
> 3. For **2-string DP**: dp size is `(N+1) × (M+1)`; row 0 / col 0 = base case.
> 4. **Pattern matching with constraints O(N)**: think KMP / Z / Manacher.
> 5. Watch **edge cases**: empty string, single char, all-same chars, unicode.
> 6. State **time & space** complexity at the end.

**You've got this! 🚀**
