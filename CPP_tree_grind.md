# 🌳 Complete Trees Interview Sheet — C++

> **The ultimate Trees guide for coding interviews & tests.**
> Binary Trees, BSTs, Traversals, AVL, Heaps, Tries, Segment Trees & more — every standard problem.

---

## 📑 Table of Contents

**FOUNDATIONS (1–8)**
1. Tree Terminology & Properties
2. Node Definitions (Binary, N-ary, Doubly)
3. Tree Traversals — Inorder, Preorder, Postorder (Recursive)
4. Iterative Traversals (Stack-based)
5. Level Order Traversal (BFS)
6. Morris Traversal — O(1) Space Inorder
7. Build Tree from Traversals
8. Serialize / Deserialize Binary Tree

**EASY (9–22)**
9. Height / Depth of Binary Tree
10. Count Nodes in Binary Tree
11. Count Leaf Nodes
12. Sum of All Nodes
13. Mirror / Invert Binary Tree
14. Check if Two Trees are Identical
15. Check Symmetric Tree
16. Diameter of Binary Tree
17. Check Balanced Binary Tree
18. Path Sum (Root to Leaf)
19. All Root-to-Leaf Paths
20. Maximum Depth & Minimum Depth
21. Sum of Left Leaves
22. Check Subtree of Another Tree

**MEDIUM (23–40)**
23. Lowest Common Ancestor (LCA) in Binary Tree
24. Lowest Common Ancestor in BST
25. Maximum Path Sum (Any Node to Any Node)
26. Right View / Left View / Top View / Bottom View
27. Zigzag (Spiral) Level Order Traversal
28. Vertical Order Traversal
29. Boundary Traversal
30. Check if Tree is BST
31. Insert / Delete in BST
32. Inorder Successor / Predecessor in BST
33. Kth Smallest / Largest in BST
34. Convert Sorted Array to Balanced BST
35. Convert Sorted Linked List to BST
36. Flatten Binary Tree to Linked List
37. Populate Next Right Pointers
38. Construct Binary Tree from Inorder + Preorder
39. Construct Binary Tree from Inorder + Postorder
40. Recover BST (Two Nodes Swapped)

**HARD (41–55)**
41. Burning Tree (Time to Burn Whole Tree)
42. Min Time to Inform All Employees / Amount of Time for Infection
43. Maximum Width of Binary Tree
44. Count Complete Tree Nodes (Optimal O(log² N))
45. Distance Between Two Nodes
46. All Nodes K Distance from Target
47. Find Duplicate Subtrees
48. Print All Paths with Given Sum
49. Sum of Numbers Formed by Root-to-Leaf Paths
50. Largest BST Subtree
51. AVL Tree — Self-balancing BST
52. Red-Black Tree Outline
53. Trie (Prefix Tree) — Insert / Search / Prefix
54. Segment Tree — Range Queries
55. Fenwick / Binary Indexed Tree

---

## ⚡ Quick Reference

| Operation | Binary Tree | BST (balanced) | BST (unbalanced) | AVL | Trie |
|-----------|-------------|----------------|------------------|-----|------|
| Search | O(N) | O(log N) | O(N) | O(log N) | O(L) |
| Insert | O(N) | O(log N) | O(N) | O(log N) | O(L) |
| Delete | O(N) | O(log N) | O(N) | O(log N) | O(L) |
| Traversal | O(N) | O(N) | O(N) | O(N) | O(N·L) |

> **L** = length of key/word.
> Most tree problems are **O(N) time, O(H) space** where H = height (O(log N) balanced, O(N) worst).

---

## 🎯 Recognizing Tree Problem Patterns

| Problem Signal | Technique |
|----------------|-----------|
| "Visit every node, compute on subtree" | Recursive DFS |
| "Level by level" | BFS with queue |
| "Path from root to leaf" | DFS with path vector |
| "Any node to any node sum/path" | DFS returning subtree value + tracking global |
| "Find ancestor of two nodes" | LCA (recursion + return) |
| "Distance between two nodes" | LCA + depth |
| "K distance / spread / burn" | Convert to graph + BFS |
| "Sorted in-order" | Property of BST |
| "Iterative inorder" | Stack |
| "O(1) space traversal" | Morris (threaded) traversal |
| "Range sum / prefix" | Segment Tree / Fenwick |
| "Prefix matching" | Trie |
| "Self-balancing" | AVL / Red-Black |
| "Top K elements / streaming" | Heap |

---

# 🟢 FOUNDATIONS

---

## 1. Tree Terminology & Properties

```
            Root
            (A)
           /   \
         (B)   (C)         ← children
         / \     \
       (D) (E)   (F)       ← leaves are D, E, F
```

| Term | Meaning |
|------|---------|
| **Root** | Top node, no parent |
| **Leaf** | Node with no children |
| **Internal** | Non-leaf node |
| **Parent / Child** | Direct connection |
| **Sibling** | Same parent |
| **Ancestor** | Any node on path to root |
| **Descendant** | Any node in subtree |
| **Depth** | Distance from root (root = 0) |
| **Height** | Longest path to leaf (leaf = 0) |
| **Subtree** | Tree formed by a node + descendants |
| **Degree** | Number of children |

**Binary tree types:**
- **Full** — every node has 0 or 2 children
- **Complete** — all levels filled except possibly last (left-filled)
- **Perfect** — every level fully filled
- **Balanced** — height difference ≤ 1 at every node
- **Degenerate** — every parent has 1 child (becomes linked list)

**Key relations (in binary trees):**
- Max nodes at level `i` = `2^i`
- Max nodes in tree of height `h` = `2^(h+1) - 1`
- Leaves L = Internal nodes I + 1 (in a full binary tree)

---

## 2. Node Definitions

```cpp
#include <bits/stdc++.h>
using namespace std;

// Binary tree node
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

// N-ary tree node
struct NaryNode {
    int val;
    vector<NaryNode*> children;
    NaryNode(int v) : val(v) {}
};

// Tree node with parent pointer
struct ParentNode {
    int val;
    ParentNode *left, *right, *parent;
    ParentNode(int v) : val(v), left(nullptr), right(nullptr), parent(nullptr) {}
};

// Tree node with next pointer (Populate Next Right)
struct NextNode {
    int val;
    NextNode *left, *right, *next;
    NextNode(int v) : val(v), left(nullptr), right(nullptr), next(nullptr) {}
};
```

---

## 3. Tree Traversals (Recursive)

```cpp
//         1
//        / \
//       2   3
//      / \
//     4   5
//
// Inorder   (Left, Root, Right):  4 2 5 1 3
// Preorder  (Root, Left, Right):  1 2 4 5 3
// Postorder (Left, Right, Root):  4 5 2 3 1

void inorder(TreeNode* root, vector<int>& out) {
    if (!root) return;
    inorder(root->left, out);
    out.push_back(root->val);
    inorder(root->right, out);
}

void preorder(TreeNode* root, vector<int>& out) {
    if (!root) return;
    out.push_back(root->val);
    preorder(root->left, out);
    preorder(root->right, out);
}

void postorder(TreeNode* root, vector<int>& out) {
    if (!root) return;
    postorder(root->left, out);
    postorder(root->right, out);
    out.push_back(root->val);
}
// TC: O(N) | SC: O(H) recursion stack
```

> **Key fact:** In-order traversal of a BST yields **sorted** values.

---

## 4. Iterative Traversals (Stack-based)

```cpp
// Iterative Inorder
vector<int> inorderIter(TreeNode* root) {
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

// Iterative Preorder
vector<int> preorderIter(TreeNode* root) {
    vector<int> out;
    if (!root) return out;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        TreeNode* cur = st.top(); st.pop();
        out.push_back(cur->val);
        if (cur->right) st.push(cur->right);   // right first
        if (cur->left)  st.push(cur->left);    // left popped first (LIFO)
    }
    return out;
}

// Iterative Postorder — trick: reversed preorder of (root → right → left)
vector<int> postorderIter(TreeNode* root) {
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

// All three in one pass — use a state marker
vector<vector<int>> allThree(TreeNode* root) {
    vector<int> pre, in, post;
    stack<pair<TreeNode*, int>> st;     // (node, state 1=pre, 2=in, 3=post)
    if (root) st.push({root, 1});
    while (!st.empty()) {
        auto& [node, state] = st.top();
        if (state == 1)      { pre.push_back(node->val); state = 2; if (node->left)  st.push({node->left, 1}); }
        else if (state == 2) { in.push_back(node->val);  state = 3; if (node->right) st.push({node->right, 1}); }
        else                 { post.push_back(node->val); st.pop(); }
    }
    return {in, pre, post};
}
// TC: O(N) | SC: O(H)
```

---

## 5. Level Order Traversal (BFS)

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();
        vector<int> level;
        for (int i = 0; i < sz; i++) {
            TreeNode* cur = q.front(); q.pop();
            level.push_back(cur->val);
            if (cur->left)  q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
        result.push_back(level);
    }
    return result;
}
// TC: O(N) | SC: O(W) where W = max width (≤ N/2)
```

---

## 6. Morris Traversal — O(1) Space Inorder

**Genius trick:** temporarily thread the rightmost node of left subtree to point to the current node, undo after visiting.

```cpp
vector<int> morrisInorder(TreeNode* root) {
    vector<int> out;
    TreeNode* cur = root;
    while (cur) {
        if (!cur->left) {
            out.push_back(cur->val);
            cur = cur->right;
        } else {
            // Find rightmost in left subtree
            TreeNode* pred = cur->left;
            while (pred->right && pred->right != cur) pred = pred->right;
            if (!pred->right) {
                pred->right = cur;            // create thread
                cur = cur->left;
            } else {
                pred->right = nullptr;        // remove thread
                out.push_back(cur->val);
                cur = cur->right;
            }
        }
    }
    return out;
}
// TC: O(N) | SC: O(1) — no recursion, no stack!
```

> **When to use:** memory-constrained environments, or when interviewer asks "can you do it in O(1) space?"

---

## 7. Build Tree from Traversals

```cpp
// From Preorder + Inorder
TreeNode* buildFromPreIn(vector<int>& pre, vector<int>& in) {
    unordered_map<int,int> inIdx;
    for (int i = 0; i < in.size(); i++) inIdx[in[i]] = i;
    int preIdx = 0;
    function<TreeNode*(int,int)> build = [&](int l, int r) -> TreeNode* {
        if (l > r) return nullptr;
        TreeNode* root = new TreeNode(pre[preIdx++]);
        int mid = inIdx[root->val];
        root->left  = build(l, mid - 1);
        root->right = build(mid + 1, r);
        return root;
    };
    return build(0, in.size() - 1);
}

// From Postorder + Inorder
TreeNode* buildFromPostIn(vector<int>& post, vector<int>& in) {
    unordered_map<int,int> inIdx;
    for (int i = 0; i < in.size(); i++) inIdx[in[i]] = i;
    int postIdx = post.size() - 1;
    function<TreeNode*(int,int)> build = [&](int l, int r) -> TreeNode* {
        if (l > r) return nullptr;
        TreeNode* root = new TreeNode(post[postIdx--]);
        int mid = inIdx[root->val];
        root->right = build(mid + 1, r);     // build RIGHT first
        root->left  = build(l, mid - 1);
        return root;
    };
    return build(0, in.size() - 1);
}
// TC: O(N) avg | SC: O(N)
```

> **Important:** can't build a unique binary tree from pre + post alone (no in-order). You need in-order to disambiguate.

---

## 8. Serialize / Deserialize Binary Tree

```cpp
class Codec {
public:
    string serialize(TreeNode* root) {
        if (!root) return "#";
        return to_string(root->val) + "," +
               serialize(root->left) + "," +
               serialize(root->right);
    }

    TreeNode* deserialize(string data) {
        stringstream ss(data);
        return build(ss);
    }
private:
    TreeNode* build(stringstream& ss) {
        string val; getline(ss, val, ',');
        if (val == "#") return nullptr;
        TreeNode* root = new TreeNode(stoi(val));
        root->left  = build(ss);
        root->right = build(ss);
        return root;
    }
};
// TC: O(N) | SC: O(N)
```

---

# 🟢 EASY PROBLEMS

---

## 9. Height / Depth of Binary Tree

```cpp
int height(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(height(root->left), height(root->right));
}
// TC: O(N) | SC: O(H)
// height of a single node = 1 (in this convention). 0 if "edges" definition.
```

---

## 10. Count Nodes in Binary Tree

```cpp
int countNodes(TreeNode* root) {
    if (!root) return 0;
    return 1 + countNodes(root->left) + countNodes(root->right);
}
// TC: O(N) | SC: O(H)
```

---

## 11. Count Leaf Nodes

```cpp
int countLeaves(TreeNode* root) {
    if (!root) return 0;
    if (!root->left && !root->right) return 1;
    return countLeaves(root->left) + countLeaves(root->right);
}
// TC: O(N) | SC: O(H)
```

---

## 12. Sum of All Nodes

```cpp
int sumOfNodes(TreeNode* root) {
    if (!root) return 0;
    return root->val + sumOfNodes(root->left) + sumOfNodes(root->right);
}
// TC: O(N) | SC: O(H)
```

---

## 13. Mirror / Invert Binary Tree

```cpp
TreeNode* invertTree(TreeNode* root) {
    if (!root) return nullptr;
    TreeNode* l = invertTree(root->left);
    TreeNode* r = invertTree(root->right);
    root->left = r;
    root->right = l;
    return root;
}
// TC: O(N) | SC: O(H)
```

---

## 14. Check if Two Trees are Identical

```cpp
bool isSameTree(TreeNode* a, TreeNode* b) {
    if (!a && !b) return true;
    if (!a || !b) return false;
    return a->val == b->val &&
           isSameTree(a->left,  b->left) &&
           isSameTree(a->right, b->right);
}
// TC: O(N) | SC: O(H)
```

---

## 15. Check Symmetric Tree

```cpp
bool isMirror(TreeNode* a, TreeNode* b) {
    if (!a && !b) return true;
    if (!a || !b) return false;
    return a->val == b->val &&
           isMirror(a->left,  b->right) &&
           isMirror(a->right, b->left);
}

bool isSymmetric(TreeNode* root) {
    return !root || isMirror(root->left, root->right);
}
// TC: O(N) | SC: O(H)
```

---

## 16. Diameter of Binary Tree

**Longest path between any two nodes (in edges or nodes).**

```cpp
int diameter = 0;

int dfsDiameter(TreeNode* root) {
    if (!root) return 0;
    int l = dfsDiameter(root->left);
    int r = dfsDiameter(root->right);
    diameter = max(diameter, l + r);      // path through this node
    return 1 + max(l, r);                  // height back to parent
}

int diameterOfBinaryTree(TreeNode* root) {
    diameter = 0;
    dfsDiameter(root);
    return diameter;                       // in edges; for nodes use diameter + 1
}
// TC: O(N) | SC: O(H)
```

---

## 17. Check Balanced Binary Tree

**Height of left and right subtrees differ by at most 1 at every node.**

```cpp
int checkBal(TreeNode* root) {
    if (!root) return 0;
    int l = checkBal(root->left);  if (l == -1) return -1;
    int r = checkBal(root->right); if (r == -1) return -1;
    if (abs(l - r) > 1) return -1;
    return 1 + max(l, r);
}

bool isBalanced(TreeNode* root) {
    return checkBal(root) != -1;
}
// TC: O(N) | SC: O(H) — early termination via -1
```

---

## 18. Path Sum (Root to Leaf)

```cpp
bool hasPathSum(TreeNode* root, int target) {
    if (!root) return false;
    if (!root->left && !root->right) return target == root->val;
    return hasPathSum(root->left,  target - root->val) ||
           hasPathSum(root->right, target - root->val);
}
// TC: O(N) | SC: O(H)
```

---

## 19. All Root-to-Leaf Paths

```cpp
void dfsPaths(TreeNode* root, vector<int>& path, vector<vector<int>>& out) {
    if (!root) return;
    path.push_back(root->val);
    if (!root->left && !root->right) out.push_back(path);
    else {
        dfsPaths(root->left,  path, out);
        dfsPaths(root->right, path, out);
    }
    path.pop_back();                       // backtrack
}

vector<vector<int>> allPaths(TreeNode* root) {
    vector<vector<int>> out;
    vector<int> path;
    dfsPaths(root, path, out);
    return out;
}
// TC: O(N × H) | SC: O(H)
```

---

## 20. Max Depth & Min Depth

```cpp
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}

int minDepth(TreeNode* root) {
    if (!root) return 0;
    if (!root->left)  return 1 + minDepth(root->right);  // must reach a leaf
    if (!root->right) return 1 + minDepth(root->left);
    return 1 + min(minDepth(root->left), minDepth(root->right));
}
// TC: O(N) | SC: O(H)
```

> **Min depth tip:** can do BFS — first leaf you hit gives the answer.

---

## 21. Sum of Left Leaves

```cpp
int sumLeftLeaves(TreeNode* root, bool isLeft = false) {
    if (!root) return 0;
    if (!root->left && !root->right) return isLeft ? root->val : 0;
    return sumLeftLeaves(root->left, true) + sumLeftLeaves(root->right, false);
}
// TC: O(N) | SC: O(H)
```

---

## 22. Check Subtree of Another Tree

```cpp
bool isSubtree(TreeNode* root, TreeNode* sub) {
    if (!root) return false;
    if (isSameTree(root, sub)) return true;
    return isSubtree(root->left, sub) || isSubtree(root->right, sub);
}
// TC: O(N × M) | SC: O(H)
// Optimal: serialize both with unique sentinels and use KMP — O(N + M)
```

---

# 🟡 MEDIUM PROBLEMS

---

## 23. Lowest Common Ancestor (LCA) in Binary Tree

```cpp
TreeNode* lca(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* l = lca(root->left,  p, q);
    TreeNode* r = lca(root->right, p, q);
    if (l && r) return root;               // p and q on different sides
    return l ? l : r;
}
// TC: O(N) | SC: O(H)
```

---

## 24. LCA in BST (Optimized using BST property)

```cpp
TreeNode* lcaBST(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root) return nullptr;
    if (p->val < root->val && q->val < root->val) return lcaBST(root->left,  p, q);
    if (p->val > root->val && q->val > root->val) return lcaBST(root->right, p, q);
    return root;                           // split point
}

// Iterative
TreeNode* lcaBSTIter(TreeNode* root, TreeNode* p, TreeNode* q) {
    while (root) {
        if (p->val < root->val && q->val < root->val) root = root->left;
        else if (p->val > root->val && q->val > root->val) root = root->right;
        else return root;
    }
    return nullptr;
}
// TC: O(H) | SC: O(1) iterative
```

---

## 25. Maximum Path Sum (Any Node to Any Node)

```cpp
int maxSum = INT_MIN;

int maxGain(TreeNode* root) {
    if (!root) return 0;
    int l = max(0, maxGain(root->left));   // ignore negative subtrees
    int r = max(0, maxGain(root->right));
    maxSum = max(maxSum, root->val + l + r);   // path through current node
    return root->val + max(l, r);              // can only extend through one side
}

int maxPathSum(TreeNode* root) {
    maxSum = INT_MIN;
    maxGain(root);
    return maxSum;
}
// TC: O(N) | SC: O(H)
```

---

## 26. Right View / Left View / Top View / Bottom View

```cpp
// Right view — last node at each level
vector<int> rightView(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    queue<TreeNode*> q; q.push(root);
    while (!q.empty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            TreeNode* cur = q.front(); q.pop();
            if (i == sz - 1) result.push_back(cur->val);
            if (cur->left)  q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
    }
    return result;
}

// Left view — first node at each level (same as above with i == 0)

// Top view — first node at each horizontal distance (HD)
vector<int> topView(TreeNode* root) {
    if (!root) return {};
    map<int,int> hdMap;            // HD -> first seen value
    queue<pair<TreeNode*,int>> q;  // (node, HD)
    q.push({root, 0});
    while (!q.empty()) {
        auto [cur, hd] = q.front(); q.pop();
        if (!hdMap.count(hd)) hdMap[hd] = cur->val;
        if (cur->left)  q.push({cur->left,  hd - 1});
        if (cur->right) q.push({cur->right, hd + 1});
    }
    vector<int> out;
    for (auto& [hd, v] : hdMap) out.push_back(v);
    return out;
}

// Bottom view — LAST node seen at each HD (just overwrite in map)
vector<int> bottomView(TreeNode* root) {
    if (!root) return {};
    map<int,int> hdMap;
    queue<pair<TreeNode*,int>> q; q.push({root, 0});
    while (!q.empty()) {
        auto [cur, hd] = q.front(); q.pop();
        hdMap[hd] = cur->val;       // overwrite — latest wins
        if (cur->left)  q.push({cur->left,  hd - 1});
        if (cur->right) q.push({cur->right, hd + 1});
    }
    vector<int> out;
    for (auto& [hd, v] : hdMap) out.push_back(v);
    return out;
}
// TC: O(N log N) due to map | SC: O(N)
```

---

## 27. Zigzag (Spiral) Level Order Traversal

```cpp
vector<vector<int>> zigzag(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    queue<TreeNode*> q; q.push(root);
    bool leftToRight = true;
    while (!q.empty()) {
        int sz = q.size();
        vector<int> level(sz);
        for (int i = 0; i < sz; i++) {
            TreeNode* cur = q.front(); q.pop();
            int idx = leftToRight ? i : sz - 1 - i;
            level[idx] = cur->val;
            if (cur->left)  q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
        result.push_back(level);
        leftToRight = !leftToRight;
    }
    return result;
}
// TC: O(N) | SC: O(W)
```

---

## 28. Vertical Order Traversal

```cpp
vector<vector<int>> verticalOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    map<int, vector<int>> hdMap;
    queue<pair<TreeNode*,int>> q; q.push({root, 0});
    while (!q.empty()) {
        auto [cur, hd] = q.front(); q.pop();
        hdMap[hd].push_back(cur->val);
        if (cur->left)  q.push({cur->left,  hd - 1});
        if (cur->right) q.push({cur->right, hd + 1});
    }
    for (auto& [hd, v] : hdMap) result.push_back(v);
    return result;
}
// TC: O(N log N) | SC: O(N)
```

---

## 29. Boundary Traversal

```cpp
bool isLeaf(TreeNode* n) { return !n->left && !n->right; }

void leftBoundary(TreeNode* root, vector<int>& out) {
    TreeNode* cur = root->left;
    while (cur) {
        if (!isLeaf(cur)) out.push_back(cur->val);
        cur = cur->left ? cur->left : cur->right;
    }
}

void leaves(TreeNode* root, vector<int>& out) {
    if (!root) return;
    if (isLeaf(root)) { out.push_back(root->val); return; }
    leaves(root->left,  out);
    leaves(root->right, out);
}

void rightBoundaryReverse(TreeNode* root, vector<int>& out) {
    vector<int> tmp;
    TreeNode* cur = root->right;
    while (cur) {
        if (!isLeaf(cur)) tmp.push_back(cur->val);
        cur = cur->right ? cur->right : cur->left;
    }
    out.insert(out.end(), tmp.rbegin(), tmp.rend());
}

vector<int> boundary(TreeNode* root) {
    vector<int> out;
    if (!root) return out;
    if (!isLeaf(root)) out.push_back(root->val);
    leftBoundary(root, out);
    leaves(root, out);
    rightBoundaryReverse(root, out);
    return out;
}
// TC: O(N) | SC: O(H)
```

---

## 30. Check if Tree is BST

```cpp
// Using bounds — correct approach
bool isBSTHelper(TreeNode* root, long minV, long maxV) {
    if (!root) return true;
    if (root->val <= minV || root->val >= maxV) return false;
    return isBSTHelper(root->left,  minV, root->val) &&
           isBSTHelper(root->right, root->val, maxV);
}

bool isBST(TreeNode* root) {
    return isBSTHelper(root, LONG_MIN, LONG_MAX);
}
// TC: O(N) | SC: O(H)

// Alternative — inorder must be strictly increasing
bool isBSTInorder(TreeNode* root) {
    stack<TreeNode*> st;
    TreeNode* cur = root;
    long prev = LONG_MIN;
    while (cur || !st.empty()) {
        while (cur) { st.push(cur); cur = cur->left; }
        cur = st.top(); st.pop();
        if (cur->val <= prev) return false;
        prev = cur->val;
        cur = cur->right;
    }
    return true;
}
```

> **Common mistake:** only comparing `root->left < root < root->right` locally. That's not enough — need to ensure ALL left descendants < root < ALL right descendants.

---

## 31. Insert / Delete in BST

```cpp
TreeNode* insertBST(TreeNode* root, int val) {
    if (!root) return new TreeNode(val);
    if (val < root->val) root->left  = insertBST(root->left,  val);
    else if (val > root->val) root->right = insertBST(root->right, val);
    return root;
}

TreeNode* findMin(TreeNode* root) {
    while (root && root->left) root = root->left;
    return root;
}

TreeNode* deleteBST(TreeNode* root, int key) {
    if (!root) return nullptr;
    if (key < root->val)      root->left  = deleteBST(root->left,  key);
    else if (key > root->val) root->right = deleteBST(root->right, key);
    else {
        // Found the node
        if (!root->left)  { TreeNode* r = root->right; delete root; return r; }
        if (!root->right) { TreeNode* l = root->left;  delete root; return l; }
        // Two children: replace with inorder successor (min of right subtree)
        TreeNode* succ = findMin(root->right);
        root->val = succ->val;
        root->right = deleteBST(root->right, succ->val);
    }
    return root;
}
// TC: O(H) | SC: O(H)
```

---

## 32. Inorder Successor / Predecessor in BST

```cpp
// Successor — smallest key > target
TreeNode* inorderSuccessor(TreeNode* root, TreeNode* target) {
    TreeNode* succ = nullptr;
    while (root) {
        if (target->val < root->val) {
            succ = root;
            root = root->left;
        } else {
            root = root->right;
        }
    }
    return succ;
}

// Predecessor — largest key < target
TreeNode* inorderPredecessor(TreeNode* root, TreeNode* target) {
    TreeNode* pred = nullptr;
    while (root) {
        if (target->val > root->val) {
            pred = root;
            root = root->right;
        } else {
            root = root->left;
        }
    }
    return pred;
}
// TC: O(H) | SC: O(1)
```

---

## 33. Kth Smallest / Largest in BST

```cpp
// Kth smallest — inorder traversal, stop at K-th
int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> st;
    TreeNode* cur = root;
    while (cur || !st.empty()) {
        while (cur) { st.push(cur); cur = cur->left; }
        cur = st.top(); st.pop();
        if (--k == 0) return cur->val;
        cur = cur->right;
    }
    return -1;
}

// Kth largest — reverse inorder (right, root, left)
int kthLargest(TreeNode* root, int k) {
    stack<TreeNode*> st;
    TreeNode* cur = root;
    while (cur || !st.empty()) {
        while (cur) { st.push(cur); cur = cur->right; }
        cur = st.top(); st.pop();
        if (--k == 0) return cur->val;
        cur = cur->left;
    }
    return -1;
}
// TC: O(H + K) | SC: O(H)
```

---

## 34. Convert Sorted Array to Balanced BST

```cpp
TreeNode* buildBalanced(vector<int>& arr, int l, int r) {
    if (l > r) return nullptr;
    int m = l + (r - l) / 2;
    TreeNode* root = new TreeNode(arr[m]);
    root->left  = buildBalanced(arr, l, m - 1);
    root->right = buildBalanced(arr, m + 1, r);
    return root;
}

TreeNode* sortedArrayToBST(vector<int>& arr) {
    return buildBalanced(arr, 0, arr.size() - 1);
}
// TC: O(N) | SC: O(log N) recursion
```

---

## 35. Convert Sorted Linked List to BST

```cpp
struct ListNode { int val; ListNode* next; };

TreeNode* sortedListToBSTHelper(ListNode*& head, int n) {
    if (n <= 0) return nullptr;
    TreeNode* left = sortedListToBSTHelper(head, n / 2);
    TreeNode* root = new TreeNode(head->val);
    head = head->next;
    root->left = left;
    root->right = sortedListToBSTHelper(head, n - n / 2 - 1);
    return root;
}

TreeNode* sortedListToBST(ListNode* head) {
    int n = 0;
    for (ListNode* c = head; c; c = c->next) n++;
    return sortedListToBSTHelper(head, n);
}
// TC: O(N) | SC: O(log N)
```

---

## 36. Flatten Binary Tree to Linked List

**In-place, preorder order, using right pointers.**

```cpp
void flatten(TreeNode* root) {
    if (!root) return;
    flatten(root->left);
    flatten(root->right);
    TreeNode* r = root->right;
    root->right = root->left;
    root->left = nullptr;
    TreeNode* cur = root;
    while (cur->right) cur = cur->right;
    cur->right = r;
}
// TC: O(N²) worst | SC: O(H)

// Morris-style O(1) extra space
void flattenIter(TreeNode* root) {
    TreeNode* cur = root;
    while (cur) {
        if (cur->left) {
            TreeNode* rightmost = cur->left;
            while (rightmost->right) rightmost = rightmost->right;
            rightmost->right = cur->right;
            cur->right = cur->left;
            cur->left = nullptr;
        }
        cur = cur->right;
    }
}
// TC: O(N) | SC: O(1)
```

---

## 37. Populate Next Right Pointers (Perfect Binary Tree)

```cpp
NextNode* connect(NextNode* root) {
    if (!root) return nullptr;
    NextNode* leftmost = root;
    while (leftmost->left) {
        NextNode* head = leftmost;
        while (head) {
            head->left->next = head->right;
            if (head->next) head->right->next = head->next->left;
            head = head->next;
        }
        leftmost = leftmost->left;
    }
    return root;
}
// TC: O(N) | SC: O(1) — uses existing 'next' pointers!
```

---

## 38–39. Construct Binary Tree from Inorder + Pre/Postorder

See `#7` — combined examples there.

---

## 40. Recover BST (Two Nodes Swapped)

**Inorder of valid BST is sorted. Two nodes swapped → at most 2 "violations" in inorder.**

```cpp
TreeNode *first = nullptr, *second = nullptr, *prev = nullptr;

void traverse(TreeNode* root) {
    if (!root) return;
    traverse(root->left);
    if (prev && prev->val > root->val) {
        if (!first) first = prev;
        second = root;
    }
    prev = root;
    traverse(root->right);
}

void recoverTree(TreeNode* root) {
    first = second = prev = nullptr;
    traverse(root);
    if (first && second) swap(first->val, second->val);
}
// TC: O(N) | SC: O(H)
```

---

# 🔴 HARD PROBLEMS

---

## 41. Burning Tree

**Tree burns from a starting node, spreads to neighbors per minute. Find total time.**

```cpp
// Step 1: build parent map. Step 2: BFS from target node.
int burnTree(TreeNode* root, int targetVal) {
    unordered_map<TreeNode*, TreeNode*> parent;
    TreeNode* target = nullptr;
    queue<TreeNode*> q; q.push(root);
    while (!q.empty()) {
        TreeNode* cur = q.front(); q.pop();
        if (cur->val == targetVal) target = cur;
        if (cur->left)  { parent[cur->left]  = cur; q.push(cur->left); }
        if (cur->right) { parent[cur->right] = cur; q.push(cur->right); }
    }
    if (!target) return -1;
    unordered_set<TreeNode*> visited;
    queue<TreeNode*> bfs;
    bfs.push(target); visited.insert(target);
    int time = -1;
    while (!bfs.empty()) {
        int sz = bfs.size();
        time++;
        for (int i = 0; i < sz; i++) {
            TreeNode* cur = bfs.front(); bfs.pop();
            for (TreeNode* nei : {cur->left, cur->right, parent.count(cur) ? parent[cur] : nullptr}) {
                if (nei && !visited.count(nei)) { visited.insert(nei); bfs.push(nei); }
            }
        }
    }
    return time;
}
// TC: O(N) | SC: O(N)
```

---

## 42. Amount of Time for Tree Infection (LeetCode 2385)

Variant of burning tree — same approach: convert to graph, BFS from start node.

```cpp
void buildGraph(TreeNode* root, unordered_map<int, vector<int>>& g) {
    if (!root) return;
    if (root->left)  { g[root->val].push_back(root->left->val);  g[root->left->val].push_back(root->val); buildGraph(root->left, g); }
    if (root->right) { g[root->val].push_back(root->right->val); g[root->right->val].push_back(root->val); buildGraph(root->right, g); }
}

int amountOfTime(TreeNode* root, int start) {
    unordered_map<int, vector<int>> g;
    buildGraph(root, g);
    unordered_set<int> seen{start};
    queue<int> q; q.push(start);
    int time = -1;
    while (!q.empty()) {
        int sz = q.size(); time++;
        for (int i = 0; i < sz; i++) {
            int cur = q.front(); q.pop();
            for (int nei : g[cur]) {
                if (!seen.count(nei)) { seen.insert(nei); q.push(nei); }
            }
        }
    }
    return time;
}
```

---

## 43. Maximum Width of Binary Tree

**Width = distance between leftmost and rightmost non-null nodes at a level (including null gaps).** Use position indices.

```cpp
int widthOfBinaryTree(TreeNode* root) {
    if (!root) return 0;
    queue<pair<TreeNode*, unsigned long long>> q;
    q.push({root, 0});
    unsigned long long maxW = 0;
    while (!q.empty()) {
        int sz = q.size();
        unsigned long long start = q.front().second, end = q.back().second;
        maxW = max(maxW, end - start + 1);
        for (int i = 0; i < sz; i++) {
            auto [cur, idx] = q.front(); q.pop();
            idx -= start;                       // normalize to avoid overflow
            if (cur->left)  q.push({cur->left,  2 * idx});
            if (cur->right) q.push({cur->right, 2 * idx + 1});
        }
    }
    return (int)maxW;
}
// TC: O(N) | SC: O(W)
```

---

## 44. Count Complete Tree Nodes — Optimal O(log² N)

```cpp
int leftHeight(TreeNode* n) {
    int h = 0; while (n) { h++; n = n->left; } return h;
}
int rightHeight(TreeNode* n) {
    int h = 0; while (n) { h++; n = n->right; } return h;
}

int countCompleteNodes(TreeNode* root) {
    if (!root) return 0;
    int lh = leftHeight(root), rh = rightHeight(root);
    if (lh == rh) return (1 << lh) - 1;           // perfect tree
    return 1 + countCompleteNodes(root->left) + countCompleteNodes(root->right);
}
// TC: O(log² N) | SC: O(log N)
```

---

## 45. Distance Between Two Nodes

```cpp
int findDepth(TreeNode* root, int target, int depth) {
    if (!root) return -1;
    if (root->val == target) return depth;
    int l = findDepth(root->left, target, depth + 1);
    return l != -1 ? l : findDepth(root->right, target, depth + 1);
}

int distance(TreeNode* root, int a, int b) {
    TreeNode *p = nullptr, *q = nullptr;
    // create dummy TreeNode* for lca — typically pass values:
    function<TreeNode*(TreeNode*,int,int)> lcaVal = [&](TreeNode* r, int x, int y) -> TreeNode* {
        if (!r || r->val == x || r->val == y) return r;
        TreeNode* l = lcaVal(r->left, x, y);
        TreeNode* rr = lcaVal(r->right, x, y);
        if (l && rr) return r;
        return l ? l : rr;
    };
    TreeNode* anc = lcaVal(root, a, b);
    return findDepth(anc, a, 0) + findDepth(anc, b, 0);
}
// TC: O(N) | SC: O(H)
```

---

## 46. All Nodes K Distance from Target

```cpp
unordered_map<TreeNode*, TreeNode*> parent;

void mapParents(TreeNode* root) {
    queue<TreeNode*> q; q.push(root);
    while (!q.empty()) {
        TreeNode* cur = q.front(); q.pop();
        if (cur->left)  { parent[cur->left]  = cur; q.push(cur->left); }
        if (cur->right) { parent[cur->right] = cur; q.push(cur->right); }
    }
}

vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
    parent.clear();
    mapParents(root);
    unordered_set<TreeNode*> seen{target};
    queue<TreeNode*> q; q.push(target);
    int dist = 0;
    while (!q.empty()) {
        if (dist == k) break;
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            TreeNode* cur = q.front(); q.pop();
            for (TreeNode* nei : {cur->left, cur->right, parent.count(cur) ? parent[cur] : nullptr}) {
                if (nei && !seen.count(nei)) { seen.insert(nei); q.push(nei); }
            }
        }
        dist++;
    }
    vector<int> result;
    while (!q.empty()) { result.push_back(q.front()->val); q.pop(); }
    return result;
}
// TC: O(N) | SC: O(N)
```

---

## 47. Find Duplicate Subtrees

```cpp
unordered_map<string, int> seen;
vector<TreeNode*> duplicates;

string serializeSub(TreeNode* root) {
    if (!root) return "#";
    string s = to_string(root->val) + "," +
               serializeSub(root->left)  + "," +
               serializeSub(root->right);
    if (++seen[s] == 2) duplicates.push_back(root);
    return s;
}

vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
    seen.clear(); duplicates.clear();
    serializeSub(root);
    return duplicates;
}
// TC: O(N²) due to string concat | SC: O(N²)
```

---

## 48. Print All Paths with Given Sum

```cpp
void pathsHelper(TreeNode* root, int target, vector<int>& path, vector<vector<int>>& out) {
    if (!root) return;
    path.push_back(root->val);
    if (!root->left && !root->right && target == root->val) out.push_back(path);
    pathsHelper(root->left,  target - root->val, path, out);
    pathsHelper(root->right, target - root->val, path, out);
    path.pop_back();
}

vector<vector<int>> pathSumII(TreeNode* root, int target) {
    vector<vector<int>> out;
    vector<int> path;
    pathsHelper(root, target, path, out);
    return out;
}
// TC: O(N × H) | SC: O(H)
```

> **Path Sum III** (paths not starting from root): use prefix-sum HashMap, O(N).

---

## 49. Sum of Root-to-Leaf Numbers

**Each path forms a number; sum them all.**

```cpp
int sumNumbers(TreeNode* root, int cur = 0) {
    if (!root) return 0;
    cur = cur * 10 + root->val;
    if (!root->left && !root->right) return cur;
    return sumNumbers(root->left, cur) + sumNumbers(root->right, cur);
}
// TC: O(N) | SC: O(H)
```

---

## 50. Largest BST Subtree

```cpp
struct Info { bool isBST; int sz, mn, mx; };

Info largestBSTHelper(TreeNode* root, int& result) {
    if (!root) return {true, 0, INT_MAX, INT_MIN};
    Info l = largestBSTHelper(root->left,  result);
    Info r = largestBSTHelper(root->right, result);
    if (l.isBST && r.isBST && root->val > l.mx && root->val < r.mn) {
        int sz = l.sz + r.sz + 1;
        result = max(result, sz);
        return {true, sz, min(l.mn, root->val), max(r.mx, root->val)};
    }
    return {false, 0, 0, 0};
}

int largestBSTSubtree(TreeNode* root) {
    int result = 0;
    largestBSTHelper(root, result);
    return result;
}
// TC: O(N) | SC: O(H)
```

---

## 51. AVL Tree — Self-Balancing BST

**Balance factor of each node = h(left) − h(right) ∈ {−1, 0, 1}.** Rotations restore balance.

```cpp
struct AVLNode {
    int val, height;
    AVLNode *left, *right;
    AVLNode(int v) : val(v), height(1), left(nullptr), right(nullptr) {}
};

int height(AVLNode* n) { return n ? n->height : 0; }
int balance(AVLNode* n) { return n ? height(n->left) - height(n->right) : 0; }
void update(AVLNode* n) { n->height = 1 + max(height(n->left), height(n->right)); }

AVLNode* rotateRight(AVLNode* y) {
    AVLNode* x = y->left;
    AVLNode* t = x->right;
    x->right = y;
    y->left = t;
    update(y); update(x);
    return x;
}

AVLNode* rotateLeft(AVLNode* x) {
    AVLNode* y = x->right;
    AVLNode* t = y->left;
    y->left = x;
    x->right = t;
    update(x); update(y);
    return y;
}

AVLNode* insertAVL(AVLNode* root, int val) {
    if (!root) return new AVLNode(val);
    if (val < root->val)      root->left  = insertAVL(root->left,  val);
    else if (val > root->val) root->right = insertAVL(root->right, val);
    else return root;
    update(root);
    int b = balance(root);
    // Left-Left
    if (b > 1 && val < root->left->val) return rotateRight(root);
    // Right-Right
    if (b < -1 && val > root->right->val) return rotateLeft(root);
    // Left-Right
    if (b > 1 && val > root->left->val)  { root->left = rotateLeft(root->left); return rotateRight(root); }
    // Right-Left
    if (b < -1 && val < root->right->val) { root->right = rotateRight(root->right); return rotateLeft(root); }
    return root;
}
// TC: O(log N) insert/search/delete | SC: O(log N)
```

---

## 52. Red-Black Tree Outline

**Self-balancing BST used in real-world libraries (`std::map`, `std::set`, Java `TreeMap`).**

**Properties:**
1. Every node is RED or BLACK.
2. Root is BLACK.
3. Red nodes can't have red children.
4. Every path from root to NULL has same number of BLACK nodes.

**Operations** (insert/delete) maintain properties via rotations + recoloring. More relaxed than AVL → fewer rotations → better for write-heavy workloads.

> **In interviews:** usually just describe properties — you won't be asked to implement.

---

## 53. Trie (Prefix Tree)

**Efficient storage & search for strings sharing prefixes.**

```cpp
class Trie {
    struct Node {
        Node* children[26] = {};
        bool end = false;
    };
    Node* root;
public:
    Trie() { root = new Node(); }

    void insert(string word) {
        Node* cur = root;
        for (char c : word) {
            int i = c - 'a';
            if (!cur->children[i]) cur->children[i] = new Node();
            cur = cur->children[i];
        }
        cur->end = true;
    }

    bool search(string word) {
        Node* cur = find(word);
        return cur && cur->end;
    }

    bool startsWith(string prefix) {
        return find(prefix) != nullptr;
    }
private:
    Node* find(string s) {
        Node* cur = root;
        for (char c : s) {
            int i = c - 'a';
            if (!cur->children[i]) return nullptr;
            cur = cur->children[i];
        }
        return cur;
    }
};
// TC: O(L) per op where L = word length | SC: O(total chars)
```

> **Use cases:** autocomplete, spell check, longest common prefix, IP routing, word search puzzles.

---

## 54. Segment Tree — Range Queries

**Range sum / min / max in O(log N), point updates in O(log N).**

```cpp
class SegmentTree {
    vector<int> tree;
    int n;

    void build(vector<int>& arr, int node, int l, int r) {
        if (l == r) { tree[node] = arr[l]; return; }
        int m = (l + r) / 2;
        build(arr, 2*node,   l,     m);
        build(arr, 2*node+1, m + 1, r);
        tree[node] = tree[2*node] + tree[2*node+1];
    }

    void update(int node, int l, int r, int idx, int val) {
        if (l == r) { tree[node] = val; return; }
        int m = (l + r) / 2;
        if (idx <= m) update(2*node,   l,     m, idx, val);
        else          update(2*node+1, m + 1, r, idx, val);
        tree[node] = tree[2*node] + tree[2*node+1];
    }

    int query(int node, int l, int r, int ql, int qr) {
        if (qr < l || r < ql) return 0;             // no overlap
        if (ql <= l && r <= qr) return tree[node];  // total overlap
        int m = (l + r) / 2;
        return query(2*node,   l,     m, ql, qr) +
               query(2*node+1, m + 1, r, ql, qr);
    }
public:
    SegmentTree(vector<int>& arr) : n(arr.size()) {
        tree.assign(4 * n, 0);
        build(arr, 1, 0, n - 1);
    }
    void update(int idx, int val) { update(1, 0, n - 1, idx, val); }
    int rangeSum(int l, int r)    { return query(1, 0, n - 1, l, r); }
};
// TC: O(log N) per op | SC: O(N)
```

> **Variants:** min/max segment tree, lazy propagation for range updates, persistent segment tree.

---

## 55. Fenwick / Binary Indexed Tree (BIT)

**Lighter than segment tree, only prefix sums + point updates. O(log N) per op, easier to code.**

```cpp
class BIT {
    vector<int> bit;
    int n;
public:
    BIT(int sz) : n(sz), bit(sz + 1, 0) {}

    void update(int i, int delta) {     // i is 1-indexed
        for (; i <= n; i += i & -i) bit[i] += delta;
    }

    int prefixSum(int i) {              // sum [1..i]
        int s = 0;
        for (; i > 0; i -= i & -i) s += bit[i];
        return s;
    }

    int rangeSum(int l, int r) {        // sum [l..r], 1-indexed
        return prefixSum(r) - prefixSum(l - 1);
    }
};
// TC: O(log N) per op | SC: O(N)
```

> **When use BIT vs Segment Tree?** BIT is simpler and faster (cache-friendly), but only handles prefix-style queries. Segment tree handles arbitrary range queries (min/max/gcd) and lazy updates.

---

# 🎯 Bonus — Quick Reference

## ⭐ Common Tree Recursion Templates

```cpp
// Pattern 1: Compute from subtrees, return up
int treeFunction(TreeNode* root) {
    if (!root) return BASE_CASE;
    int l = treeFunction(root->left);
    int r = treeFunction(root->right);
    return combine(root->val, l, r);
}

// Pattern 2: Pass info DOWN (parent state)
void dfs(TreeNode* root, int parentInfo) {
    if (!root) return;
    int newInfo = update(parentInfo, root->val);
    // do work with newInfo
    dfs(root->left,  newInfo);
    dfs(root->right, newInfo);
}

// Pattern 3: Track best globally + return for parent
int bestGlobal;
int dfs(TreeNode* root) {
    if (!root) return 0;
    int l = max(0, dfs(root->left));
    int r = max(0, dfs(root->right));
    bestGlobal = max(bestGlobal, l + r + root->val);   // path through current
    return root->val + max(l, r);                       // only ONE arm goes up
}

// Pattern 4: Return rich info from subtree
struct Info { /* multiple fields */ };
Info dfs(TreeNode* root) {
    if (!root) return baseCase;
    Info l = dfs(root->left), r = dfs(root->right);
    return combineInfo(root, l, r);
}
```

---

## ⭐ Top 10 Must-Know

1. **Recursive traversals** — base for almost every tree problem
2. **Level-order BFS** with queue + size loop
3. **DFS with global/passed state**
4. **Lowest Common Ancestor** (both BT and BST)
5. **Diameter / max path sum** — return one arm, track global both arms
6. **Path-sum problems** — backtracking with `vector<int>`
7. **Build tree from traversals** — inorder + (pre/post) with hash map
8. **Serialize / Deserialize** — preorder with null markers
9. **Convert tree problem to graph** — burning, K-distance, infection
10. **Tries / Segment Trees / BIT** — specialized for prefix / range queries

---

## ⭐ Common Interview Questions

> **Q: BFS vs DFS for trees — when to use which?**
> A: BFS (queue) for level-by-level problems, shortest path/depth to leaf, level views. DFS (recursion or stack) for subtree computations, path sums, traversals, most "compute over subtree" problems.

> **Q: Why is in-order important for BSTs?**
> A: In-order traversal yields sorted values. Most BST validation, kth-smallest, predecessor/successor problems exploit this.

> **Q: Difference between Complete, Full, and Perfect binary tree?**
> A: **Full** — every node has 0 or 2 children. **Complete** — filled level by level, last level left-aligned. **Perfect** — all leaves at same depth and every internal node has 2 children (also full + complete).

> **Q: How to validate a BST correctly?**
> A: Use **bounds** (`minV` < node < `maxV`) recursively, NOT just `left < root < right` (that doesn't catch deep violations). Or do in-order traversal and verify strictly increasing.

> **Q: How does Morris traversal achieve O(1) space?**
> A: Temporarily creates "threads" — sets the rightmost node of left subtree to point back to current node. Walks left subtree, returns via thread, removes thread, moves right.

> **Q: AVL vs Red-Black tree?**
> A: AVL is more strictly balanced (faster lookups). RB allows more imbalance (fewer rotations on insert/delete, better for write-heavy). C++ `std::map`, Java `TreeMap`, Linux kernel use RB.

> **Q: Why does a BST become O(N) in the worst case?**
> A: If inserts are sorted (1, 2, 3, ...), the tree degenerates into a linked list (height = N). Self-balancing trees (AVL, RB) prevent this.

> **Q: When use a Trie instead of a HashSet for words?**
> A: When you need prefix operations (autocomplete, "startsWith"), longest common prefix, or want O(L) regardless of dataset size. HashSet is O(1) average for exact match only.

> **Q: How would you find LCA in a Binary Tree (not BST)?**
> A: Recurse — if root is p or q, return root. Recurse both sides. If both return non-null, root is LCA. Else return whichever is non-null. O(N).

> **Q: Segment Tree vs BIT (Fenwick)?**
> A: BIT is lighter, faster, easier — prefix sums + point updates. Segment Tree handles arbitrary range queries (min/max/etc.) and supports lazy propagation for range updates.

> **Q: How to serialize a tree?**
> A: Preorder traversal with `#` for nulls. Reconstruct by recursing through tokens with same order. Works for any binary tree.

> **Q: How to find K-th distance nodes in O(N)?**
> A: Build parent pointers via one BFS/DFS pass. Then BFS outward from the target (in 3 directions: left, right, parent). Stops at level K.

> **Q: How do you flatten a tree to linked list in O(1) space?**
> A: Morris-like — for each node, find rightmost node in left subtree, attach right subtree there, move left subtree to right.

> **Q: Iterative inorder — explain the algorithm.**
> A: Use a stack. Walk left as far as possible, pushing nodes. Pop, visit, move to right child. Repeat until stack empty and current is null.

---

## ⭐ Common Pitfalls

✅ **Null check** is the #1 cause of bugs — always handle `root == nullptr`.
✅ **Don't confuse height/depth conventions** — clarify with interviewer.
✅ **Validating BST locally** (`left < root < right`) is WRONG — use bounds.
✅ **Forgetting to backtrack** in path problems — `path.pop_back()` is essential.
✅ **Negative numbers** in path sum — initialize `maxSum = INT_MIN`, allow `max(0, ...)` for subtree contributions.
✅ **Integer overflow** in BST bounds — use `long long`.
✅ **Modifying tree while iterating** — risky. Capture nodes first.
✅ **N-ary trees:** can't just use binary recursion — iterate children list.
✅ **Recursion stack overflow** for skewed trees (height = N) — switch to iterative.
✅ **Memory leaks** — `delete` nodes when removing them from BST.

---

## ⭐ Practice Problems

| LeetCode | Concept |
|----------|---------|
| 94, 144, 145 | Inorder, Preorder, Postorder |
| 102, 107 | Level order |
| 103 | Zigzag |
| 199 | Right view |
| 104, 111 | Max/min depth |
| 226 | Invert tree |
| 100, 101 | Same / Symmetric |
| 543 | Diameter |
| 110 | Balanced |
| 112, 113 | Path sum I, II |
| 124 | Max path sum |
| 236 | LCA Binary Tree |
| 235 | LCA BST |
| 98 | Validate BST |
| 230 | Kth smallest in BST |
| 108, 109 | Sorted array/list → BST |
| 105, 106 | Build from traversals |
| 297 | Serialize / Deserialize |
| 114 | Flatten to linked list |
| 116, 117 | Populate next right |
| 99 | Recover BST |
| 222 | Count complete tree nodes |
| 662 | Maximum width |
| 863 | Nodes at K distance |
| 2385 | Tree infection time |
| 208 | Trie |
| 307 | Range sum (Segment tree / BIT) |

---

# 💪 GO ACE THAT TREE QUESTION!

> **Test-day strategy:**
> 1. **Draw the tree first** — 90% of bugs come from misunderstanding structure.
> 2. **Identify the pattern** — subtree computation / level-by-level / path / search.
> 3. **Pick recursion template** (one of the 4 above).
> 4. **Always handle null** — first line of every recursive function.
> 5. **For BST problems** — exploit the sorted-inorder property.
> 6. **For "any node to any node"** — DFS that returns one arm + tracks both arms globally.
> 7. **For graph-like problems on trees** — convert to graph + BFS.
> 8. **State complexity** — usually O(N) time, O(H) space.
>
> **You've got this! 🌳🚀**
