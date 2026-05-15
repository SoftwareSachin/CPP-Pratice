# 📚 Complete Stack Interview Sheet — C++

> **The ultimate Stack guide for coding interviews & tests.**
> Basics, monotonic stacks, parentheses, expression evaluation, design problems — every standard pattern.

---

## 📑 Table of Contents

**FOUNDATIONS (1–8)**
1. Stack Concept & ADT
2. Stack Using Array
3. Stack Using Linked List
4. Stack Using STL (`std::stack`)
5. Stack Using Two Queues
6. Queue Using Two Stacks
7. Stack Using Single Queue
8. Reverse a Stack (Recursion)

**EASY (9–20)**
9. Valid Parentheses
10. Min Stack (O(1) getMin)
11. Max Stack
12. Implement Stack Using std::vector
13. Baseball Game (LeetCode 682)
14. Backspace String Compare
15. Build an Array With Stack Operations
16. Make The String Great
17. Remove All Adjacent Duplicates
18. Reverse a String Using Stack
19. Sort a Stack (Recursion)
20. Insert at Bottom of Stack (Recursion)

**MONOTONIC STACK (21–32)**
21. Monotonic Stack — Pattern & Template
22. Next Greater Element I
23. Next Greater Element II (Circular)
24. Next Smaller Element
25. Previous Greater / Smaller Element
26. Stock Span Problem
27. Daily Temperatures
28. Largest Rectangle in Histogram
29. Maximal Rectangle (2D)
30. Trapping Rain Water (Stack solution)
31. Sum of Subarray Minimums
32. Sum of Subarray Ranges

**EXPRESSION EVALUATION (33–40)**
33. Evaluate Reverse Polish Notation
34. Infix to Postfix Conversion
35. Postfix to Infix
36. Evaluate Infix Expression
37. Basic Calculator I (+, -, parentheses)
38. Basic Calculator II (+, -, *, /)
39. Basic Calculator III (full)
40. Decode String

**ITERATIVE TRAVERSALS (41–45)**
41. Iterative Inorder Traversal
42. Iterative Preorder Traversal
43. Iterative Postorder Traversal (1 + 2 stack methods)
44. DFS on Graph using Stack
45. Flatten Nested List Iterator

**ADVANCED & HARD (46–58)**
46. Asteroid Collision
47. Remove K Digits
48. 132 Pattern
49. Online Stock Span (Streaming)
50. Maximum Frequency Stack
51. Simplify Path (Unix-style)
52. Validate Stack Sequences
53. Score of Parentheses
54. Minimum Add to Make Parentheses Valid
55. Longest Valid Parentheses
56. Remove Invalid Parentheses
57. Exclusive Time of Functions
58. Smallest Subsequence of Distinct Characters

---

## ⚡ Quick Reference

| Operation | Time |
|-----------|------|
| `push` | O(1) |
| `pop`  | O(1) |
| `top` / `peek` | O(1) |
| `empty` | O(1) |
| `size` | O(1) |
| Search in stack | O(N) |

**Stack = LIFO (Last In, First Out).**

---

## 🎯 When to Use a Stack

| Problem Signal | Pattern |
|----------------|---------|
| "Matching pairs", "valid parentheses" | Stack-based matching |
| "Next greater / smaller / span" | **Monotonic stack** |
| "Evaluate expression" | Operator/operand stacks |
| "Undo / backtrack" | Stack of states |
| "Iterative DFS" | Replace recursion with stack |
| "Nested structure" | Stack tracks current scope |
| "Histogram / skyline / rectangle" | Monotonic stack |
| "Sum of subarray min/max" | Contribution via monotonic stack |
| "Reverse / mirror" | Push everything, pop reversed |
| "Min/max tracked alongside" | Two stacks or pair-stack |

---

# 🟢 FOUNDATIONS

---

## 1. Stack Concept & ADT

```
Push 3 → Push 1 → Push 4 → Pop → Pop → Push 9

       │   │   │   │   │ 4 │   │ 1 │   │   │   │ 9 │
       │   │   │ 1 │   │ 1 │   │ 3 │   │ 3 │   │ 3 │
       │ 3 │   │ 3 │   │ 3 │   │   │   │   │   │   │
       └───┘   └───┘   └───┘   └───┘   └───┘   └───┘
```

**Operations:**
- `push(x)` — add to top
- `pop()` — remove from top
- `top()` / `peek()` — return top without removing
- `empty()` — check if empty
- `size()` — number of elements

**LIFO** — last pushed is first popped.

---

## 2. Stack Using Array (Fixed Capacity)

```cpp
class ArrayStack {
    vector<int> data;
    int capacity;
public:
    ArrayStack(int cap = 1000) : capacity(cap) {}

    bool push(int x) {
        if (data.size() == capacity) return false;     // overflow
        data.push_back(x);
        return true;
    }

    bool pop() {
        if (data.empty()) return false;                // underflow
        data.pop_back();
        return true;
    }

    int top() {
        return data.empty() ? -1 : data.back();
    }

    bool empty() { return data.empty(); }
    int  size()  { return data.size(); }
};
// All ops: O(1) | SC: O(N)
```

---

## 3. Stack Using Linked List (Dynamic)

```cpp
class LinkedStack {
    struct Node { int val; Node* next; Node(int v) : val(v), next(nullptr) {} };
    Node* head = nullptr;
    int sz = 0;
public:
    void push(int x) {
        Node* n = new Node(x);
        n->next = head;
        head = n;
        sz++;
    }

    bool pop() {
        if (!head) return false;
        Node* tmp = head;
        head = head->next;
        delete tmp;
        sz--;
        return true;
    }

    int top() { return head ? head->val : -1; }
    bool empty() { return head == nullptr; }
    int  size()  { return sz; }

    ~LinkedStack() { while (head) pop(); }
};
// All ops: O(1) | SC: O(N) — grows dynamically
```

---

## 4. Stack Using STL — `std::stack`

```cpp
#include <stack>
stack<int> st;

st.push(10);
st.push(20);
st.top();          // 20
st.pop();          // doesn't return — call top() first
st.empty();
st.size();

// Common idioms
while (!st.empty()) { int x = st.top(); st.pop(); /* use x */ }

// Stack of pairs / structs
stack<pair<int,int>> pst;
pst.push({3, 5});
auto [a, b] = pst.top();
```

> **`std::stack` is a container adapter** — wraps `deque` by default. Use `std::stack<int, vector<int>>` if you prefer vector backing.

---

## 5. Stack Using Two Queues

```cpp
class StackWith2Queues {
    queue<int> q1, q2;
public:
    void push(int x) {
        q2.push(x);
        while (!q1.empty()) { q2.push(q1.front()); q1.pop(); }
        swap(q1, q2);
    }
    void pop()  { q1.pop(); }
    int  top()  { return q1.front(); }
    bool empty() { return q1.empty(); }
};
// push: O(N) | pop / top: O(1)
```

```cpp
// One-queue version (push expensive too, but uses one queue)
class StackWithOneQueue {
    queue<int> q;
public:
    void push(int x) {
        q.push(x);
        for (int i = 0; i < (int)q.size() - 1; i++) {
            q.push(q.front()); q.pop();
        }
    }
    void pop()  { q.pop(); }
    int  top()  { return q.front(); }
    bool empty() { return q.empty(); }
};
```

---

## 6. Queue Using Two Stacks

**Push to in-stack; pop/peek from out-stack (refill from in when empty).**

```cpp
class QueueWith2Stacks {
    stack<int> in, out;
    void shift() {
        while (!in.empty()) { out.push(in.top()); in.pop(); }
    }
public:
    void push(int x) { in.push(x); }

    int pop() {
        if (out.empty()) shift();
        int v = out.top(); out.pop();
        return v;
    }

    int peek() {
        if (out.empty()) shift();
        return out.top();
    }

    bool empty() { return in.empty() && out.empty(); }
};
// Amortized O(1) per op
```

---

## 7. Stack Using Single Queue (covered above in #5)

---

## 8. Reverse a Stack Using Recursion (No Extra Data Structure)

```cpp
void insertAtBottom(stack<int>& st, int x) {
    if (st.empty()) { st.push(x); return; }
    int top = st.top(); st.pop();
    insertAtBottom(st, x);
    st.push(top);
}

void reverseStack(stack<int>& st) {
    if (st.empty()) return;
    int top = st.top(); st.pop();
    reverseStack(st);
    insertAtBottom(st, top);
}
// TC: O(N²) | SC: O(N) recursion
```

---

# 🟢 EASY PROBLEMS

---

## 9. Valid Parentheses (LeetCode 20)

```cpp
bool isValid(string s) {
    stack<char> st;
    unordered_map<char,char> mp = {{')','('}, {']','['}, {'}','{'}};
    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') st.push(c);
        else {
            if (st.empty() || st.top() != mp[c]) return false;
            st.pop();
        }
    }
    return st.empty();
}
// TC: O(N) | SC: O(N)
```

---

## 10. Min Stack — O(1) getMin

```cpp
class MinStack {
    stack<int> st;
    stack<int> mins;
public:
    void push(int x) {
        st.push(x);
        if (mins.empty() || x <= mins.top()) mins.push(x);
    }
    void pop() {
        if (st.top() == mins.top()) mins.pop();
        st.pop();
    }
    int top() { return st.top(); }
    int getMin() { return mins.top(); }
};
// All ops: O(1) | SC: O(N)
```

```cpp
// Space-optimized (single stack, encode pairs)
class MinStackOptim {
    stack<pair<int,int>> st;        // {value, min so far}
public:
    void push(int x) {
        int mn = st.empty() ? x : min(st.top().second, x);
        st.push({x, mn});
    }
    void pop() { st.pop(); }
    int top() { return st.top().first; }
    int getMin() { return st.top().second; }
};
```

---

## 11. Max Stack (LeetCode 716)

**Like Min Stack — but supports `popMax()` too. Two stacks (or one with embedded max).**

```cpp
class MaxStack {
    stack<int> st, maxes;
public:
    void push(int x) {
        st.push(x);
        maxes.push(maxes.empty() ? x : max(maxes.top(), x));
    }
    void pop() { st.pop(); maxes.pop(); }
    int top() { return st.top(); }
    int peekMax() { return maxes.top(); }
    int popMax() {
        int mx = maxes.top();
        stack<int> tmp;
        while (st.top() != mx) { tmp.push(st.top()); pop(); }
        pop();
        while (!tmp.empty()) { push(tmp.top()); tmp.pop(); }
        return mx;
    }
};
// push, pop, top, peekMax: O(1) | popMax: O(N)
```

---

## 12. Implement Stack Using std::vector (already done — see #2)

---

## 13. Baseball Game (LeetCode 682)

```cpp
int calPoints(vector<string>& ops) {
    vector<int> st;
    for (auto& s : ops) {
        if (s == "+") {
            int n = st.size();
            st.push_back(st[n-1] + st[n-2]);
        } else if (s == "D") {
            st.push_back(2 * st.back());
        } else if (s == "C") {
            st.pop_back();
        } else {
            st.push_back(stoi(s));
        }
    }
    return accumulate(st.begin(), st.end(), 0);
}
// TC: O(N) | SC: O(N)
```

---

## 14. Backspace String Compare

```cpp
string build(string& s) {
    string st;
    for (char c : s) {
        if (c == '#') { if (!st.empty()) st.pop_back(); }
        else st.push_back(c);
    }
    return st;
}

bool backspaceCompare(string s, string t) {
    return build(s) == build(t);
}
// TC: O(N + M) | SC: O(N + M)
// O(1) space: walk both from end with skip counter
```

---

## 15. Build Array With Stack Operations (LeetCode 1441)

```cpp
vector<string> buildArray(vector<int>& target, int n) {
    vector<string> ops;
    int j = 0;
    for (int i = 1; i <= n && j < target.size(); i++) {
        ops.push_back("Push");
        if (target[j] == i) j++;
        else ops.push_back("Pop");
    }
    return ops;
}
// TC: O(N) | SC: O(N)
```

---

## 16. Make The String Great (LeetCode 1544)

**Remove adjacent same letter pairs of opposite case.**

```cpp
string makeGood(string s) {
    string st;
    for (char c : s) {
        if (!st.empty() && abs(st.back() - c) == 32) st.pop_back();
        else st.push_back(c);
    }
    return st;
}
// TC: O(N) | SC: O(N)
```

---

## 17. Remove All Adjacent Duplicates

```cpp
// I — remove pairs (LeetCode 1047)
string removeDuplicates(string s) {
    string st;
    for (char c : s) {
        if (!st.empty() && st.back() == c) st.pop_back();
        else st.push_back(c);
    }
    return st;
}

// II — remove K-tuples (LeetCode 1209)
string removeDuplicatesII(string s, int k) {
    vector<pair<char,int>> st;          // {char, count}
    for (char c : s) {
        if (!st.empty() && st.back().first == c) {
            if (++st.back().second == k) st.pop_back();
        } else {
            st.push_back({c, 1});
        }
    }
    string result;
    for (auto& [c, cnt] : st) result.append(cnt, c);
    return result;
}
// TC: O(N) | SC: O(N)
```

---

## 18. Reverse a String Using Stack

```cpp
string reverseString(string s) {
    stack<char> st;
    for (char c : s) st.push(c);
    string out;
    while (!st.empty()) { out += st.top(); st.pop(); }
    return out;
}
// TC: O(N) | SC: O(N)
```

---

## 19. Sort a Stack Using Recursion

```cpp
void insertSorted(stack<int>& st, int x) {
    if (st.empty() || st.top() <= x) { st.push(x); return; }
    int top = st.top(); st.pop();
    insertSorted(st, x);
    st.push(top);
}

void sortStack(stack<int>& st) {
    if (st.empty()) return;
    int top = st.top(); st.pop();
    sortStack(st);
    insertSorted(st, top);
}
// TC: O(N²) | SC: O(N) recursion
```

---

## 20. Insert at Bottom (see #8)

---

# 🟡 MONOTONIC STACK

---

## 21. Monotonic Stack — Pattern & Template

**A stack that maintains a strictly increasing or decreasing order.** Common use: find next/previous greater/smaller element in O(N).

### Why O(N)?
Each element pushed and popped **at most once** → amortized linear.

### When to Use
- "Next greater / smaller element"
- "Span" problems (how many elements before X are smaller?)
- "Largest rectangle"
- "Subarray min/max contribution"

### Template — Next Greater Element

```cpp
// For each i: find next index j > i with arr[j] > arr[i]
vector<int> nextGreater(vector<int>& a) {
    int n = a.size();
    vector<int> ans(n, -1);          // -1 if no greater exists
    stack<int> st;                    // store INDICES; values decreasing top-to-bottom
    for (int i = 0; i < n; i++) {
        while (!st.empty() && a[st.top()] < a[i]) {
            ans[st.top()] = a[i];
            st.pop();
        }
        st.push(i);
    }
    return ans;
}
// TC: O(N) | SC: O(N)
```

### Variants — same template, change comparison and direction:

| Want | Direction | Comparison | Stack maintains |
|------|-----------|------------|-----------------|
| Next Greater | left → right | `a[top] < a[i]` | Decreasing |
| Next Smaller | left → right | `a[top] > a[i]` | Increasing |
| Previous Greater | right → left OR pop-when-current-bigger | swap order | Decreasing |
| Previous Smaller | right → left OR pop-when-current-smaller | swap order | Increasing |

---

## 22. Next Greater Element I (LeetCode 496)

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int,int> mp;
    stack<int> st;
    for (int x : nums2) {
        while (!st.empty() && st.top() < x) { mp[st.top()] = x; st.pop(); }
        st.push(x);
    }
    vector<int> ans;
    for (int x : nums1) ans.push_back(mp.count(x) ? mp[x] : -1);
    return ans;
}
// TC: O(N + M) | SC: O(N)
```

---

## 23. Next Greater Element II (Circular, LeetCode 503)

**Trick: iterate `2*N` times, use `i % N`.**

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n, -1);
    stack<int> st;
    for (int i = 0; i < 2 * n; i++) {
        int x = nums[i % n];
        while (!st.empty() && nums[st.top()] < x) {
            ans[st.top()] = x;
            st.pop();
        }
        if (i < n) st.push(i);
    }
    return ans;
}
// TC: O(N) | SC: O(N)
```

---

## 24. Next Smaller Element

```cpp
vector<int> nextSmaller(vector<int>& a) {
    int n = a.size();
    vector<int> ans(n, -1);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && a[st.top()] > a[i]) {
            ans[st.top()] = a[i];
            st.pop();
        }
        st.push(i);
    }
    return ans;
}
// TC: O(N) | SC: O(N)
```

---

## 25. Previous Greater / Smaller Element

```cpp
// Previous greater: scan left → right, stack maintains decreasing
vector<int> previousGreater(vector<int>& a) {
    int n = a.size();
    vector<int> ans(n, -1);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && a[st.top()] <= a[i]) st.pop();
        if (!st.empty()) ans[i] = a[st.top()];
        st.push(i);
    }
    return ans;
}

// Previous smaller — flip the comparison
vector<int> previousSmaller(vector<int>& a) {
    int n = a.size();
    vector<int> ans(n, -1);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && a[st.top()] >= a[i]) st.pop();
        if (!st.empty()) ans[i] = a[st.top()];
        st.push(i);
    }
    return ans;
}
// TC: O(N) | SC: O(N)
```

---

## 26. Stock Span Problem (LeetCode 901 / GFG)

**Span = number of consecutive days BEFORE today with price ≤ today's price.**

```cpp
vector<int> stockSpan(vector<int>& prices) {
    int n = prices.size();
    vector<int> span(n, 1);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && prices[st.top()] <= prices[i]) st.pop();
        span[i] = st.empty() ? (i + 1) : (i - st.top());
        st.push(i);
    }
    return span;
}
// TC: O(N) | SC: O(N)
```

---

## 27. Daily Temperatures (LeetCode 739)

**For each day: how many days until a warmer one?**

```cpp
vector<int> dailyTemperatures(vector<int>& T) {
    int n = T.size();
    vector<int> ans(n, 0);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && T[st.top()] < T[i]) {
            ans[st.top()] = i - st.top();
            st.pop();
        }
        st.push(i);
    }
    return ans;
}
// TC: O(N) | SC: O(N)
```

---

## 28. Largest Rectangle in Histogram (LeetCode 84)

**Classic monotonic stack — the one to memorize.**

```cpp
int largestRectangleArea(vector<int>& h) {
    int n = h.size(), best = 0;
    stack<int> st;
    for (int i = 0; i <= n; i++) {
        int cur = (i == n) ? 0 : h[i];     // sentinel: forces flush at end
        while (!st.empty() && h[st.top()] > cur) {
            int height = h[st.top()]; st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            best = max(best, height * width);
        }
        st.push(i);
    }
    return best;
}
// TC: O(N) | SC: O(N)
```

**Intuition:** for each bar, find the widest range where it's the minimum.

---

## 29. Maximal Rectangle (LeetCode 85)

**Treat each row's running heights as a histogram. Apply #28 per row.**

```cpp
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

## 30. Trapping Rain Water — Stack Solution (LeetCode 42)

**Use monotonic decreasing stack. When a bar taller than top arrives, pop and compute water above.**

```cpp
int trap(vector<int>& h) {
    int n = h.size(), water = 0;
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && h[st.top()] < h[i]) {
            int bottom = h[st.top()]; st.pop();
            if (st.empty()) break;
            int width = i - st.top() - 1;
            int height = min(h[st.top()], h[i]) - bottom;
            water += width * height;
        }
        st.push(i);
    }
    return water;
}
// TC: O(N) | SC: O(N)
```

---

## 31. Sum of Subarray Minimums (LeetCode 907)

**For each `a[i]`, count subarrays where it's the minimum. Sum contributions.**

```cpp
int sumSubarrayMins(vector<int>& a) {
    const int MOD = 1e9 + 7;
    int n = a.size();
    vector<int> prevSmaller(n, -1), nextSmaller(n, n);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && a[st.top()] > a[i]) {
            nextSmaller[st.top()] = i;
            st.pop();
        }
        prevSmaller[i] = st.empty() ? -1 : st.top();
        st.push(i);
    }
    long long sum = 0;
    for (int i = 0; i < n; i++) {
        long long left  = i - prevSmaller[i];
        long long right = nextSmaller[i] - i;
        sum = (sum + a[i] * left * right) % MOD;
    }
    return (int)sum;
}
// TC: O(N) | SC: O(N)
// Careful with strict vs non-strict to avoid double-counting equal mins.
```

---

## 32. Sum of Subarray Ranges (LeetCode 2104)

**Sum of (max − min) over all subarrays = Sum(max contributions) − Sum(min contributions).**

```cpp
long long subArrayRanges(vector<int>& a) {
    int n = a.size();
    auto contribute = [&](bool isMin) {
        vector<int> prev(n, -1), next(n, n);
        stack<int> st;
        for (int i = 0; i < n; i++) {
            while (!st.empty() && (isMin ? a[st.top()] > a[i] : a[st.top()] < a[i])) {
                next[st.top()] = i; st.pop();
            }
            prev[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }
        long long sum = 0;
        for (int i = 0; i < n; i++) {
            long long left  = i - prev[i];
            long long right = next[i] - i;
            sum += (long long)a[i] * left * right;
        }
        return sum;
    };
    return contribute(false) - contribute(true);
}
// TC: O(N) | SC: O(N)
```

---

# 🧮 EXPRESSION EVALUATION

---

## 33. Evaluate Reverse Polish Notation (LeetCode 150)

```cpp
int evalRPN(vector<string>& tokens) {
    stack<long long> st;
    for (auto& t : tokens) {
        if (t == "+" || t == "-" || t == "*" || t == "/") {
            long long b = st.top(); st.pop();
            long long a = st.top(); st.pop();
            if (t == "+") st.push(a + b);
            else if (t == "-") st.push(a - b);
            else if (t == "*") st.push(a * b);
            else st.push(a / b);
        } else {
            st.push(stoll(t));
        }
    }
    return (int)st.top();
}
// TC: O(N) | SC: O(N)
```

---

## 34. Infix to Postfix Conversion (Shunting-Yard)

```cpp
int prec(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    if (op == '^') return 3;
    return 0;
}

string infixToPostfix(string s) {
    string out;
    stack<char> ops;
    for (char c : s) {
        if (isalnum(c)) out += c;
        else if (c == '(') ops.push(c);
        else if (c == ')') {
            while (!ops.empty() && ops.top() != '(') { out += ops.top(); ops.pop(); }
            ops.pop();                       // pop '('
        } else {                             // operator
            while (!ops.empty() && ops.top() != '(' && prec(ops.top()) >= prec(c)) {
                out += ops.top(); ops.pop();
            }
            ops.push(c);
        }
    }
    while (!ops.empty()) { out += ops.top(); ops.pop(); }
    return out;
}
// TC: O(N) | SC: O(N)
```

---

## 35. Postfix to Infix

```cpp
string postfixToInfix(string s) {
    stack<string> st;
    for (char c : s) {
        if (isalnum(c)) st.push(string(1, c));
        else {
            string b = st.top(); st.pop();
            string a = st.top(); st.pop();
            st.push("(" + a + c + b + ")");
        }
    }
    return st.top();
}
// TC: O(N) | SC: O(N)
```

---

## 36. Evaluate Infix Expression

```cpp
int applyOp(int a, int b, char op) {
    switch(op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
    }
    return 0;
}

int evaluateInfix(string s) {
    stack<int> vals;
    stack<char> ops;
    int i = 0, n = s.size();
    while (i < n) {
        if (s[i] == ' ') { i++; continue; }
        if (isdigit(s[i])) {
            int num = 0;
            while (i < n && isdigit(s[i])) { num = num * 10 + (s[i] - '0'); i++; }
            vals.push(num);
        } else if (s[i] == '(') { ops.push(s[i]); i++; }
        else if (s[i] == ')') {
            while (ops.top() != '(') {
                int b = vals.top(); vals.pop();
                int a = vals.top(); vals.pop();
                vals.push(applyOp(a, b, ops.top())); ops.pop();
            }
            ops.pop(); i++;
        } else {                             // operator
            while (!ops.empty() && ops.top() != '(' && prec(ops.top()) >= prec(s[i])) {
                int b = vals.top(); vals.pop();
                int a = vals.top(); vals.pop();
                vals.push(applyOp(a, b, ops.top())); ops.pop();
            }
            ops.push(s[i]); i++;
        }
    }
    while (!ops.empty()) {
        int b = vals.top(); vals.pop();
        int a = vals.top(); vals.pop();
        vals.push(applyOp(a, b, ops.top())); ops.pop();
    }
    return vals.top();
}
// TC: O(N) | SC: O(N)
```

---

## 37. Basic Calculator I (LeetCode 224 — +, -, parentheses)

```cpp
int calculate(string s) {
    stack<int> st;
    int result = 0, num = 0, sign = 1;
    for (char c : s) {
        if (isdigit(c)) num = num * 10 + (c - '0');
        else if (c == '+' || c == '-') {
            result += sign * num;
            num = 0;
            sign = (c == '+') ? 1 : -1;
        } else if (c == '(') {
            st.push(result); st.push(sign);
            result = 0; sign = 1;
        } else if (c == ')') {
            result += sign * num; num = 0;
            result *= st.top(); st.pop();      // sign before (
            result += st.top(); st.pop();      // result before (
        }
    }
    return result + sign * num;
}
// TC: O(N) | SC: O(N)
```

---

## 38. Basic Calculator II (LeetCode 227 — +, -, *, /, no parens)

```cpp
int calculateII(string s) {
    stack<int> st;
    int num = 0;
    char op = '+';
    for (int i = 0; i <= s.size(); i++) {
        char c = (i == s.size()) ? '+' : s[i];
        if (isdigit(c)) num = num * 10 + (c - '0');
        else if (c != ' ') {
            if      (op == '+') st.push(num);
            else if (op == '-') st.push(-num);
            else if (op == '*') { int t = st.top(); st.pop(); st.push(t * num); }
            else                { int t = st.top(); st.pop(); st.push(t / num); }
            num = 0; op = c;
        }
    }
    int sum = 0;
    while (!st.empty()) { sum += st.top(); st.pop(); }
    return sum;
}
// TC: O(N) | SC: O(N)
```

---

## 39. Basic Calculator III (LeetCode 772 — all operators + parens)

**Combine #37 + #38: recursion / parsing with full operator precedence.**

```cpp
class Solution {
    int i = 0;
    int calc(string& s) {
        stack<int> st;
        int num = 0;
        char op = '+';
        while (i < s.size() && s[i] != ')') {
            char c = s[i++];
            if (isdigit(c)) {
                num = num * 10 + (c - '0');
                while (i < s.size() && isdigit(s[i])) num = num * 10 + (s[i++] - '0');
            } else if (c == '(') {
                num = calc(s);
                if (i < s.size() && s[i] == ')') i++;
            }
            if (!isdigit(c) && c != ' ' && c != '(') {
                if      (op == '+') st.push(num);
                else if (op == '-') st.push(-num);
                else if (op == '*') { int t = st.top(); st.pop(); st.push(t * num); }
                else                { int t = st.top(); st.pop(); st.push(t / num); }
                num = 0; op = c;
            }
        }
        // Final number
        if      (op == '+') st.push(num);
        else if (op == '-') st.push(-num);
        else if (op == '*') { int t = st.top(); st.pop(); st.push(t * num); }
        else                { int t = st.top(); st.pop(); st.push(t / num); }
        int sum = 0;
        while (!st.empty()) { sum += st.top(); st.pop(); }
        return sum;
    }
public:
    int calculate(string s) { i = 0; return calc(s); }
};
// TC: O(N) | SC: O(N)
```

---

## 40. Decode String (LeetCode 394)

**`3[a2[c]]` → `accaccacc`.**

```cpp
string decodeString(string s) {
    stack<int> counts;
    stack<string> strs;
    string cur;
    int k = 0;
    for (char c : s) {
        if (isdigit(c)) {
            k = k * 10 + (c - '0');
        } else if (c == '[') {
            counts.push(k);
            strs.push(cur);
            cur.clear();
            k = 0;
        } else if (c == ']') {
            string decoded = strs.top(); strs.pop();
            int times = counts.top(); counts.pop();
            for (int i = 0; i < times; i++) decoded += cur;
            cur = decoded;
        } else {
            cur += c;
        }
    }
    return cur;
}
// TC: O(N × maxK) | SC: O(N)
```

---

# 🌳 ITERATIVE TRAVERSALS

---

## 41. Iterative Inorder

```cpp
struct TreeNode { int val; TreeNode *left, *right; };

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> out;
    stack<TreeNode*> st;
    TreeNode* cur = root;
    while (cur || !st.empty()) {
        while (cur) { st.push(cur); cur = cur->left; }
        cur = st.top(); st.pop();
        out.push_back(cur->val);
        cur = cur->right;
    }
    return out;
}
// TC: O(N) | SC: O(H)
```

---

## 42. Iterative Preorder

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> out;
    if (!root) return out;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        TreeNode* cur = st.top(); st.pop();
        out.push_back(cur->val);
        if (cur->right) st.push(cur->right);
        if (cur->left)  st.push(cur->left);
    }
    return out;
}
// TC: O(N) | SC: O(H)
```

---

## 43. Iterative Postorder (Two Methods)

```cpp
// Method 1: reverse of "root-right-left"
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> out;
    if (!root) return out;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        TreeNode* cur = st.top(); st.pop();
        out.push_back(cur->val);
        if (cur->left)  st.push(cur->left);
        if (cur->right) st.push(cur->right);
    }
    reverse(out.begin(), out.end());
    return out;
}

// Method 2: true postorder with one stack + lastVisited tracking
vector<int> postorderTrue(TreeNode* root) {
    vector<int> out;
    stack<TreeNode*> st;
    TreeNode *cur = root, *lastVisited = nullptr;
    while (cur || !st.empty()) {
        while (cur) { st.push(cur); cur = cur->left; }
        TreeNode* peek = st.top();
        if (peek->right && lastVisited != peek->right) {
            cur = peek->right;
        } else {
            out.push_back(peek->val);
            lastVisited = peek;
            st.pop();
        }
    }
    return out;
}
// Both TC: O(N) | SC: O(H)
```

---

## 44. DFS on Graph Using Stack

```cpp
void dfsIter(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);
    stack<int> st;
    st.push(start);
    while (!st.empty()) {
        int u = st.top(); st.pop();
        if (visited[u]) continue;
        visited[u] = true;
        cout << u << " ";
        for (int v : adj[u]) {
            if (!visited[v]) st.push(v);
        }
    }
}
// TC: O(V + E) | SC: O(V)
```

> **Use when:** deep graphs would overflow recursion stack.

---

## 45. Flatten Nested List Iterator (LeetCode 341)

```cpp
class NestedInteger { /* given API */ };

class NestedIterator {
    stack<pair<vector<NestedInteger>*, int>> st;     // {list, idx}
    void advance() {
        while (!st.empty()) {
            auto& [v, idx] = st.top();
            if (idx == (int)v->size()) { st.pop(); continue; }
            if ((*v)[idx].isInteger()) return;
            // Else descend into nested list
            auto* sub = &(*v)[idx].getList();
            idx++;
            st.push({sub, 0});
        }
    }
public:
    NestedIterator(vector<NestedInteger>& nestedList) {
        st.push({&nestedList, 0});
        advance();
    }
    int next() {
        int v = st.top().first->at(st.top().second).getInteger();
        st.top().second++;
        advance();
        return v;
    }
    bool hasNext() { return !st.empty(); }
};
// Amortized O(1) per next/hasNext | SC: O(D) depth
```

---

# 🔴 ADVANCED & HARD

---

## 46. Asteroid Collision (LeetCode 735)

**Positive moves right, negative moves left. Larger absolute survives; equal both die.**

```cpp
vector<int> asteroidCollision(vector<int>& a) {
    vector<int> st;
    for (int x : a) {
        bool destroyed = false;
        while (!st.empty() && x < 0 && st.back() > 0) {
            if (st.back() < -x)      st.pop_back();
            else if (st.back() == -x) { st.pop_back(); destroyed = true; break; }
            else                     { destroyed = true; break; }
        }
        if (!destroyed) st.push_back(x);
    }
    return st;
}
// TC: O(N) | SC: O(N)
```

---

## 47. Remove K Digits (LeetCode 402)

**Smallest number after removing K digits.** Use monotonic increasing stack.

```cpp
string removeKdigits(string num, int k) {
    string st;
    for (char c : num) {
        while (k > 0 && !st.empty() && st.back() > c) { st.pop_back(); k--; }
        st.push_back(c);
    }
    while (k-- > 0 && !st.empty()) st.pop_back();
    int i = 0;
    while (i < st.size() && st[i] == '0') i++;
    string result = st.substr(i);
    return result.empty() ? "0" : result;
}
// TC: O(N) | SC: O(N)
```

---

## 48. 132 Pattern (LeetCode 456)

**Find i < j < k with `a[i] < a[k] < a[j]`.** Scan right → left, maintain max value below the current top.

```cpp
bool find132pattern(vector<int>& nums) {
    int n = nums.size();
    int third = INT_MIN;                     // the "2" in 132 pattern
    stack<int> st;
    for (int i = n - 1; i >= 0; i--) {
        if (nums[i] < third) return true;
        while (!st.empty() && nums[i] > st.top()) {
            third = st.top(); st.pop();
        }
        st.push(nums[i]);
    }
    return false;
}
// TC: O(N) | SC: O(N)
```

---

## 49. Online Stock Span (LeetCode 901)

**Streaming version of stock span — answer per `next(price)` call.**

```cpp
class StockSpanner {
    stack<pair<int,int>> st;             // {price, span}
public:
    int next(int price) {
        int span = 1;
        while (!st.empty() && st.top().first <= price) {
            span += st.top().second;
            st.pop();
        }
        st.push({price, span});
        return span;
    }
};
// Amortized O(1) | SC: O(N) total
```

---

## 50. Maximum Frequency Stack (LeetCode 895)

**Pop the most frequent; ties: most recently pushed.**

```cpp
class FreqStack {
    unordered_map<int,int> freq;
    unordered_map<int, stack<int>> group;
    int maxFreq = 0;
public:
    void push(int x) {
        int f = ++freq[x];
        maxFreq = max(maxFreq, f);
        group[f].push(x);
    }
    int pop() {
        int x = group[maxFreq].top(); group[maxFreq].pop();
        if (group[maxFreq].empty()) maxFreq--;
        freq[x]--;
        return x;
    }
};
// All ops: O(1) | SC: O(N)
```

---

## 51. Simplify Path — Unix-style (LeetCode 71)

```cpp
string simplifyPath(string path) {
    vector<string> st;
    stringstream ss(path);
    string token;
    while (getline(ss, token, '/')) {
        if (token == "" || token == ".") continue;
        if (token == "..") { if (!st.empty()) st.pop_back(); }
        else st.push_back(token);
    }
    string result;
    for (auto& s : st) result += "/" + s;
    return result.empty() ? "/" : result;
}
// TC: O(N) | SC: O(N)
```

---

## 52. Validate Stack Sequences (LeetCode 946)

**Given push order + pop order, can a stack produce them?** Simulate.

```cpp
bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
    stack<int> st;
    int j = 0;
    for (int x : pushed) {
        st.push(x);
        while (!st.empty() && j < popped.size() && st.top() == popped[j]) {
            st.pop(); j++;
        }
    }
    return st.empty();
}
// TC: O(N) | SC: O(N)
```

---

## 53. Score of Parentheses (LeetCode 856)

**`()` → 1, `AB` → A + B, `(A)` → 2A.**

```cpp
int scoreOfParentheses(string s) {
    stack<int> st;
    st.push(0);                              // current frame's score
    for (char c : s) {
        if (c == '(') st.push(0);
        else {
            int inner = st.top(); st.pop();
            int add = inner == 0 ? 1 : 2 * inner;
            st.top() += add;
        }
    }
    return st.top();
}
// TC: O(N) | SC: O(N)

// O(1) space — count depth at each ')'
int scoreOfParensO1(string s) {
    int score = 0, depth = 0;
    for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(') depth++;
        else {
            depth--;
            if (s[i-1] == '(') score += 1 << depth;
        }
    }
    return score;
}
```

---

## 54. Minimum Add to Make Parentheses Valid (LeetCode 921)

```cpp
int minAddToMakeValid(string s) {
    int open = 0, close = 0;
    for (char c : s) {
        if (c == '(') open++;
        else {
            if (open > 0) open--;
            else close++;
        }
    }
    return open + close;
}
// TC: O(N) | SC: O(1) — counters instead of stack
```

---

## 55. Longest Valid Parentheses (LeetCode 32)

```cpp
int longestValidParentheses(string s) {
    stack<int> st;
    st.push(-1);                             // sentinel
    int best = 0;
    for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(') st.push(i);
        else {
            st.pop();
            if (st.empty()) st.push(i);
            else best = max(best, i - st.top());
        }
    }
    return best;
}
// TC: O(N) | SC: O(N)
```

---

## 56. Remove Invalid Parentheses (LeetCode 301)

**Minimum removals to make string valid; return all valid results.** Two-pass + DFS.

```cpp
class Solution {
    vector<string> result;
    void dfs(string s, int start, int lastBracket, vector<char> par) {
        int count = 0;
        for (int i = start; i < s.size(); i++) {
            if (s[i] == par[0]) count++;
            else if (s[i] == par[1]) count--;
            if (count >= 0) continue;
            for (int j = lastBracket; j <= i; j++) {
                if (s[j] == par[1] && (j == lastBracket || s[j-1] != par[1])) {
                    dfs(s.substr(0, j) + s.substr(j + 1), i, j, par);
                }
            }
            return;
        }
        string rev = string(s.rbegin(), s.rend());
        if (par[0] == '(') dfs(rev, 0, 0, {')', '('});
        else result.push_back(rev);
    }
public:
    vector<string> removeInvalidParentheses(string s) {
        dfs(s, 0, 0, {'(', ')'});
        return result;
    }
};
// TC: O(2^N) worst | SC: O(N)
```

---

## 57. Exclusive Time of Functions (LeetCode 636)

```cpp
vector<int> exclusiveTime(int n, vector<string>& logs) {
    vector<int> res(n, 0);
    stack<int> st;                           // function ids
    int prevTime = 0;
    for (auto& log : logs) {
        int colon1 = log.find(':');
        int colon2 = log.rfind(':');
        int id   = stoi(log.substr(0, colon1));
        string type = log.substr(colon1 + 1, colon2 - colon1 - 1);
        int time = stoi(log.substr(colon2 + 1));
        if (type == "start") {
            if (!st.empty()) res[st.top()] += time - prevTime;
            st.push(id);
            prevTime = time;
        } else {
            res[st.top()] += time - prevTime + 1;
            st.pop();
            prevTime = time + 1;
        }
    }
    return res;
}
// TC: O(L) | SC: O(N)
```

---

## 58. Smallest Subsequence of Distinct Characters (LeetCode 1081 / 316)

**Lexicographically smallest subsequence containing every char exactly once.**

```cpp
string smallestSubsequence(string s) {
    int cnt[26] = {0};
    bool inStack[26] = {false};
    for (char c : s) cnt[c - 'a']++;
    string st;
    for (char c : s) {
        cnt[c - 'a']--;
        if (inStack[c - 'a']) continue;
        while (!st.empty() && st.back() > c && cnt[st.back() - 'a'] > 0) {
            inStack[st.back() - 'a'] = false;
            st.pop_back();
        }
        st.push_back(c);
        inStack[c - 'a'] = true;
    }
    return st;
}
// TC: O(N) | SC: O(1) — fixed alphabet
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Top 10 Must-Know

1. **Valid Parentheses** — the entry-level matching problem
2. **Min Stack** — auxiliary stack technique
3. **Monotonic stack template** — next/previous greater/smaller
4. **Largest Rectangle in Histogram** — the king of stack problems
5. **Daily Temperatures** — classic monotonic
6. **Trapping Rain Water** (stack solution)
7. **Iterative tree traversals**
8. **Evaluate RPN** + **Basic Calculator I/II**
9. **Decode String** — nested-scope handling
10. **Asteroid Collision** — simulation with stack

---

## ⭐ Monotonic Stack — Mental Model

```
Want next greater?
  - stack stores INDICES, values monotonically decreasing (top is smallest)
  - while current > stack top, pop and record current as their "next greater"
  - push current index

Want next smaller?
  - swap comparisons (>, <)
  - stack maintains increasing

Want previous greater?
  - scan same direction, just READ the stack top before pushing
  - pop until top is greater than current

Memorize this one decision flow — covers 80% of stack problems.
```

---

## ⭐ Common Interview Questions

> **Q: Stack vs Queue — when use which?**
> A: Stack (LIFO) for backtracking, parsing, undo, reverse, DFS. Queue (FIFO) for BFS, scheduling, level traversal, anything order-preserving.

> **Q: How does `std::stack` work internally?**
> A: It's a **container adapter** wrapping `std::deque` by default. You can choose the underlying container — `std::stack<int, std::vector<int>>` is also valid.

> **Q: What's a monotonic stack?**
> A: A stack whose elements (or values at indexed elements) are kept in monotonic (strictly increasing/decreasing) order. Achieves O(N) for "next greater/smaller" type problems because each element is pushed/popped at most once.

> **Q: How do you implement a min-stack with O(1) getMin?**
> A: Maintain a parallel "min stack" tracking the minimum seen so far. On push, also push min(x, currentMin). On pop, also pop. Top of min stack is always current min.

> **Q: Why O(N) for histogram rectangle problem?**
> A: Each bar is pushed and popped exactly once. When popped, we compute the rectangle where it was the limiting height. Amortized O(1) per bar.

> **Q: Why use a stack for iterative DFS instead of recursion?**
> A: Avoids stack overflow on deep graphs (N > 10⁵). Same algorithmic behavior. Recursion = implicit stack.

> **Q: Difference between iterative inorder using stack and Morris traversal?**
> A: Stack uses O(H) extra space. Morris uses O(1) by temporarily threading nodes. Both O(N) time.

> **Q: How to evaluate an expression with parentheses?**
> A: Two-stack approach (operands + operators), or Shunting-Yard to convert to postfix then evaluate. Or recurse on '(' and ')'.

> **Q: Can you implement a queue with two stacks?**
> A: Yes. Push to in-stack; for pop/peek, transfer everything to out-stack if it's empty, then take from out-stack. Amortized O(1).

> **Q: How does the 132 pattern algorithm work in O(N)?**
> A: Scan right to left maintaining a stack of candidates for "3" and a variable tracking the max "2" we've seen. If we ever find a number less than the "2", we've completed the 132 pattern.

> **Q: When is a stack the WRONG choice?**
> A: When you need access in arbitrary order (use array), priority order (use heap), or FIFO (use queue). Also when constant-time arbitrary lookup is required.

> **Q: How do you detect balanced brackets in O(1) space?**
> A: For just one type — use counter. For multiple types — must use a stack to remember which bracket type is open.

---

## ⭐ Common Pitfalls

✅ **Calling `top()` on empty stack** is undefined behavior — always check `empty()` first.
✅ **`pop()` returns void in C++** — call `top()` first to retrieve the value.
✅ **Iteration direction matters** in monotonic stack — left→right for "next" questions, right→left for "previous".
✅ **Strict vs non-strict comparisons** in monotonic stack — affects duplicate handling in sum-of-subarray-minimums.
✅ **Sentinel values** simplify histogram-style problems — append `0` to force final flush.
✅ **Using `int` for products of large counts** — `1e9 + 1e9` overflows. Use `long long`.
✅ **Wrong direction in postfix conversion** — operators stack output is in reverse of intuition.
✅ **`std::stack` is not iterable** — can't loop through; only access top.
✅ **Don't forget to clear stack** between test cases in competitive programming.
✅ **Recursive solutions for deep stacks** — risk stack overflow; use iterative.

---

## ⭐ Decision Tree

```
Need to track most-recent-first?
└─ Yes → Stack

Need to match brackets / pairs?
└─ Yes → Stack

Find next/previous greater/smaller in O(N)?
└─ Yes → Monotonic stack

Evaluate expression?
└─ Yes → Operand + operator stacks (or postfix conversion)

Iterative tree/graph traversal?
└─ Yes → Stack replaces recursion

Need O(1) min / max alongside push/pop?
└─ Yes → Auxiliary stack

Anything else?
└─ Probably not a stack problem.
```

---

## ⭐ Practice Problems

| LeetCode | Pattern |
|----------|---------|
| 20 | Valid parens |
| 155 | Min stack |
| 232, 225 | Queue ↔ Stack |
| 150 | RPN evaluation |
| 224, 227, 772 | Calculator I/II/III |
| 394 | Decode string |
| 71 | Simplify path |
| 1047, 1209 | Adjacent duplicates |
| 682 | Baseball game |
| 735 | Asteroid collision |
| 402 | Remove K digits |
| 316, 1081 | Smallest subsequence distinct |
| 496, 503 | Next greater I & II |
| 739 | Daily temperatures |
| 901 | Online stock span |
| 84 | Largest histogram rectangle |
| 85 | Maximal rectangle |
| 42 | Trapping rain water |
| 907 | Sum of subarray min |
| 2104 | Sum of subarray ranges |
| 456 | 132 pattern |
| 895 | Max frequency stack |
| 946 | Validate stack sequences |
| 856 | Score of parentheses |
| 921, 32, 301 | Parenthesis fix problems |
| 636 | Exclusive function time |
| 341 | Flatten nested list iterator |
| 94, 144, 145 | Iterative tree traversals |

---

# 💪 GO ACE THAT STACK QUESTION!

> **Test-day strategy:**
> 1. **Recognize the pattern** first — monotonic? matching? expression? simulation?
> 2. **What do you store on the stack?** Indices? Values? Pairs? State?
> 3. **What pops when?** Define the pop condition clearly.
> 4. **Sentinel trick** for histogram-style problems — append a 0/sentinel at end.
> 5. **State complexity** — most stack problems are O(N) time, O(N) space.
> 6. **Iterative > recursive** for deep inputs — avoid stack overflow.
> 7. **Auxiliary stack** for O(1) min/max alongside operations.
>
> **You've got this! 📚🚀**
