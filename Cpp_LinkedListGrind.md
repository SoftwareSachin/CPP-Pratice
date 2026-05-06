# 🔗 Complete Linked List Interview Sheet — C++

> **The ultimate Linked List guide for coding interviews & tests.**
> Singly, Doubly, Circular — Reverse, Cycle, Merge, Sort, Clone, LRU & more.

---

## 📑 Table of Contents

**FOUNDATIONS (1–8)**
1. Node Definitions (Singly, Doubly, Circular)
2. Traverse / Print Linked List
3. Insertion (Head, Tail, Position)
4. Deletion (Head, Tail, Position, By Value)
5. Length of Linked List
6. Search Element in Linked List
7. Convert Array to Linked List & Vice Versa
8. Get Nth Node from Beginning

**EASY (9–25)**
9. Reverse a Linked List (Iterative + Recursive)
10. Find Middle of Linked List
11. Detect Cycle in Linked List (Floyd's)
12. Find Start of Cycle
13. Length of Cycle in Linked List
14. Remove Cycle from Linked List
15. Find Nth Node from End
16. Delete Nth Node from End
17. Check Palindrome Linked List
18. Merge Two Sorted Linked Lists
19. Remove Duplicates from Sorted Linked List
20. Remove Duplicates from Unsorted Linked List
21. Intersection of Two Linked Lists (Y-shape)
22. Add 1 to a Number Represented as Linked List
23. Delete Given Node (without head pointer)
24. Move Last Element to Front
25. Count Occurrences of an Element

**MEDIUM (26–42)**
26. Add Two Numbers as Linked Lists
27. Reverse Linked List in Groups of K
28. Reverse Nodes in Pairs (Swap Pairs)
29. Rotate a Linked List by K
30. Odd-Even Linked List
31. Partition Linked List around X
32. Sort 0s, 1s, 2s Linked List
33. Remove Nth Node from End (one pass)
34. Reorder List (L0→Ln→L1→Ln-1→...)
35. Split a Linked List into K Parts
36. Flatten a Multilevel Doubly Linked List
37. Clone Linked List with Random Pointer
38. Swap Kth Node from Beginning and End
39. Subtract Two Numbers (LL representation)
40. Multiply Two Numbers (LL representation)
41. Segregate Even and Odd Nodes
42. Insert in Sorted Linked List

**HARD (43–55)**
43. Merge K Sorted Lists
44. Sort a Linked List (Merge Sort) — O(N log N)
45. Quick Sort on Linked List
46. Flatten a Linked List (with bottom pointers)
47. LRU Cache (Doubly Linked List + HashMap)
48. LFU Cache (Linked Lists + HashMaps)
49. Reverse Doubly Linked List
50. Detect & Remove Loop in Doubly Linked List
51. Clone Linked List with Random Pointer (O(1) extra)
52. Convert Binary Tree to Doubly Linked List (Inorder)
53. Convert Sorted Linked List to Balanced BST
54. Design Linked List from Scratch
55. Skip List Implementation Outline

---

## ⚡ Quick Complexity Reference

| Operation | Time | Notes |
|-----------|------|-------|
| Traversal | O(N) | Always linear |
| Search | O(N) | Sequential only |
| Insert at head | O(1) | Update head pointer |
| Insert at tail | O(N) or O(1) | O(1) with tail pointer |
| Insert at position | O(N) | Need to traverse |
| Delete head/tail | O(1) / O(N) | DLL: O(1) tail |
| Reverse | O(N) | 3 pointers |
| Cycle detect | O(N) | Floyd's slow/fast |
| Merge sorted | O(N+M) | Two pointers |
| Merge sort | O(N log N) | Divide & conquer |

---

# 🟢 FOUNDATIONS

---

## 1. Node Definitions

```cpp
#include <bits/stdc++.h>
using namespace std;

// Singly Linked List Node
struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

// Doubly Linked List Node
struct DNode {
    int data;
    DNode* prev;
    DNode* next;
    DNode(int val) : data(val), prev(nullptr), next(nullptr) {}
};

// Node with random pointer (clone problem)
struct RNode {
    int data;
    RNode* next;
    RNode* random;
    RNode(int val) : data(val), next(nullptr), random(nullptr) {}
};
```

---

## 2. Traverse / Print Linked List

```cpp
void printList(Node* head) {
    Node* cur = head;
    while (cur) {
        cout << cur->data;
        if (cur->next) cout << " -> ";
        cur = cur->next;
    }
    cout << "\n";
}
// TC: O(N) | SC: O(1)
```

---

## 3. Insertion (Head, Tail, Position)

```cpp
// Insert at head
Node* insertHead(Node* head, int val) {
    Node* node = new Node(val);
    node->next = head;
    return node;
}

// Insert at tail
Node* insertTail(Node* head, int val) {
    Node* node = new Node(val);
    if (!head) return node;
    Node* cur = head;
    while (cur->next) cur = cur->next;
    cur->next = node;
    return head;
}

// Insert at position (0-indexed)
Node* insertAt(Node* head, int pos, int val) {
    if (pos == 0) return insertHead(head, val);
    Node* cur = head;
    for (int i = 0; i < pos - 1 && cur; i++) cur = cur->next;
    if (!cur) return head;  // position out of range
    Node* node = new Node(val);
    node->next = cur->next;
    cur->next = node;
    return head;
}
// TC: O(1) head, O(N) tail/position | SC: O(1)
```

---

## 4. Deletion (Head, Tail, Position, By Value)

```cpp
// Delete head
Node* deleteHead(Node* head) {
    if (!head) return nullptr;
    Node* tmp = head;
    head = head->next;
    delete tmp;
    return head;
}

// Delete tail
Node* deleteTail(Node* head) {
    if (!head || !head->next) { delete head; return nullptr; }
    Node* cur = head;
    while (cur->next->next) cur = cur->next;
    delete cur->next;
    cur->next = nullptr;
    return head;
}

// Delete at position
Node* deleteAt(Node* head, int pos) {
    if (!head) return nullptr;
    if (pos == 0) return deleteHead(head);
    Node* cur = head;
    for (int i = 0; i < pos - 1 && cur; i++) cur = cur->next;
    if (!cur || !cur->next) return head;
    Node* tmp = cur->next;
    cur->next = tmp->next;
    delete tmp;
    return head;
}

// Delete by value (first occurrence)
Node* deleteValue(Node* head, int val) {
    if (!head) return nullptr;
    if (head->data == val) return deleteHead(head);
    Node* cur = head;
    while (cur->next && cur->next->data != val) cur = cur->next;
    if (cur->next) {
        Node* tmp = cur->next;
        cur->next = tmp->next;
        delete tmp;
    }
    return head;
}
// TC: O(N) | SC: O(1)
```

---

## 5. Length of Linked List

```cpp
int length(Node* head) {
    int count = 0;
    while (head) { count++; head = head->next; }
    return count;
}
// TC: O(N) | SC: O(1)
```

---

## 6. Search Element in Linked List

```cpp
bool search(Node* head, int target) {
    while (head) {
        if (head->data == target) return true;
        head = head->next;
    }
    return false;
}
// TC: O(N) | SC: O(1)
```

---

## 7. Convert Array ↔ Linked List

```cpp
Node* arrayToList(vector<int>& arr) {
    Node dummy(0);
    Node* cur = &dummy;
    for (int x : arr) {
        cur->next = new Node(x);
        cur = cur->next;
    }
    return dummy.next;
}

vector<int> listToArray(Node* head) {
    vector<int> result;
    while (head) { result.push_back(head->data); head = head->next; }
    return result;
}
// TC: O(N) | SC: O(N)
```

---

## 8. Get Nth Node from Beginning

```cpp
Node* getNth(Node* head, int n) {  // 1-indexed
    Node* cur = head;
    for (int i = 1; i < n && cur; i++) cur = cur->next;
    return cur;
}
// TC: O(N) | SC: O(1)
```

---

# 🟢 EASY PROBLEMS

---

## 9. Reverse a Linked List

```cpp
// Iterative — three pointers
Node* reverseIter(Node* head) {
    Node *prev = nullptr, *cur = head;
    while (cur) {
        Node* next = cur->next;
        cur->next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}

// Recursive
Node* reverseRec(Node* head) {
    if (!head || !head->next) return head;
    Node* rest = reverseRec(head->next);
    head->next->next = head;
    head->next = nullptr;
    return rest;
}
// TC: O(N) | SC: O(1) iter, O(N) recursive (stack)
```

---

## 10. Find Middle of Linked List

**Slow / fast pointer.**

```cpp
Node* middle(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
    // For even-length list, returns the SECOND middle.
    // For first middle: change condition to `fast->next && fast->next->next`.
}
// TC: O(N) | SC: O(1)
```

---

## 11. Detect Cycle in Linked List — Floyd's Tortoise & Hare

```cpp
bool hasCycle(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
// TC: O(N) | SC: O(1)
```

---

## 12. Find Start of Cycle

**After meeting, move one pointer to head; both move 1 step until they meet.**

```cpp
Node* cycleStart(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow->next;
                fast = fast->next;
            }
            return slow;
        }
    }
    return nullptr;
}
// TC: O(N) | SC: O(1)
```

---

## 13. Length of Cycle in Linked List

```cpp
int cycleLength(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            int len = 1;
            Node* cur = slow->next;
            while (cur != slow) { cur = cur->next; len++; }
            return len;
        }
    }
    return 0;
}
// TC: O(N) | SC: O(1)
```

---

## 14. Remove Cycle from Linked List

```cpp
void removeCycle(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) break;
    }
    if (!fast || !fast->next) return;       // no cycle
    slow = head;
    if (slow == fast) {                      // cycle starts at head
        while (fast->next != slow) fast = fast->next;
    } else {
        while (slow->next != fast->next) {
            slow = slow->next;
            fast = fast->next;
        }
    }
    fast->next = nullptr;
}
// TC: O(N) | SC: O(1)
```

---

## 15. Find Nth Node from End

```cpp
Node* nthFromEnd(Node* head, int n) {
    Node *fast = head, *slow = head;
    for (int i = 0; i < n; i++) {
        if (!fast) return nullptr;       // n > length
        fast = fast->next;
    }
    while (fast) { slow = slow->next; fast = fast->next; }
    return slow;
}
// TC: O(N) | SC: O(1)
```

---

## 16. Delete Nth Node from End

**Use dummy node to handle "delete head" cleanly.**

```cpp
Node* deleteNthFromEnd(Node* head, int n) {
    Node dummy(0);
    dummy.next = head;
    Node *fast = &dummy, *slow = &dummy;
    for (int i = 0; i < n; i++) fast = fast->next;
    while (fast->next) { fast = fast->next; slow = slow->next; }
    Node* tmp = slow->next;
    slow->next = tmp->next;
    delete tmp;
    return dummy.next;
}
// TC: O(N) | SC: O(1)
```

---

## 17. Check Palindrome Linked List

**Approach:** find middle → reverse second half → compare → restore.

```cpp
bool isPalindrome(Node* head) {
    if (!head || !head->next) return true;
    // 1. Find middle (first middle for even)
    Node *slow = head, *fast = head;
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    // 2. Reverse second half
    Node* second = reverseIter(slow->next);
    Node *p1 = head, *p2 = second;
    bool result = true;
    while (p2) {
        if (p1->data != p2->data) { result = false; break; }
        p1 = p1->next; p2 = p2->next;
    }
    // 3. (Optional) Restore
    slow->next = reverseIter(second);
    return result;
}
// TC: O(N) | SC: O(1)
```

---

## 18. Merge Two Sorted Linked Lists

```cpp
Node* mergeTwoSorted(Node* a, Node* b) {
    Node dummy(0);
    Node* tail = &dummy;
    while (a && b) {
        if (a->data <= b->data) { tail->next = a; a = a->next; }
        else                    { tail->next = b; b = b->next; }
        tail = tail->next;
    }
    tail->next = a ? a : b;
    return dummy.next;
}
// TC: O(N + M) | SC: O(1)

// Recursive
Node* mergeRec(Node* a, Node* b) {
    if (!a) return b;
    if (!b) return a;
    if (a->data <= b->data) { a->next = mergeRec(a->next, b); return a; }
    else                    { b->next = mergeRec(a, b->next); return b; }
}
```

---

## 19. Remove Duplicates from Sorted Linked List

```cpp
Node* removeDuplicatesSorted(Node* head) {
    Node* cur = head;
    while (cur && cur->next) {
        if (cur->data == cur->next->data) {
            Node* tmp = cur->next;
            cur->next = tmp->next;
            delete tmp;
        } else cur = cur->next;
    }
    return head;
}
// TC: O(N) | SC: O(1)
```

---

## 20. Remove Duplicates from Unsorted Linked List

```cpp
Node* removeDuplicatesUnsorted(Node* head) {
    unordered_set<int> seen;
    Node dummy(0);
    dummy.next = head;
    Node* prev = &dummy;
    Node* cur = head;
    while (cur) {
        if (seen.count(cur->data)) {
            prev->next = cur->next;
            delete cur;
            cur = prev->next;
        } else {
            seen.insert(cur->data);
            prev = cur;
            cur = cur->next;
        }
    }
    return dummy.next;
}
// TC: O(N) avg | SC: O(N)
```

---

## 21. Intersection of Two Linked Lists (Y-shape)

**Trick:** switch heads when one pointer reaches end. Both walk N+M and meet at intersection or null.

```cpp
Node* getIntersection(Node* a, Node* b) {
    if (!a || !b) return nullptr;
    Node *p1 = a, *p2 = b;
    while (p1 != p2) {
        p1 = p1 ? p1->next : b;
        p2 = p2 ? p2->next : a;
    }
    return p1;
}
// TC: O(N + M) | SC: O(1)
```

---

## 22. Add 1 to a Number Represented as Linked List

**Reverse → add 1 with carry → reverse back. Or use recursion from the end.**

```cpp
Node* addOne(Node* head) {
    head = reverseIter(head);
    Node* cur = head;
    int carry = 1;
    while (cur && carry) {
        int sum = cur->data + carry;
        cur->data = sum % 10;
        carry = sum / 10;
        if (carry && !cur->next) {
            cur->next = new Node(carry);
            carry = 0;
            break;
        }
        cur = cur->next;
    }
    return reverseIter(head);
}
// TC: O(N) | SC: O(1)
```

---

## 23. Delete Given Node (Without Head Pointer)

**Trick:** copy next node's data into current, then unlink next node. Won't work for tail.

```cpp
void deleteGivenNode(Node* node) {
    if (!node || !node->next) return;
    Node* nxt = node->next;
    node->data = nxt->data;
    node->next = nxt->next;
    delete nxt;
}
// TC: O(1) | SC: O(1)
```

---

## 24. Move Last Element to Front

```cpp
Node* moveLastToFront(Node* head) {
    if (!head || !head->next) return head;
    Node* secondLast = head;
    while (secondLast->next->next) secondLast = secondLast->next;
    Node* last = secondLast->next;
    secondLast->next = nullptr;
    last->next = head;
    return last;
}
// TC: O(N) | SC: O(1)
```

---

## 25. Count Occurrences of an Element

```cpp
int countOccurrences(Node* head, int target) {
    int count = 0;
    while (head) {
        if (head->data == target) count++;
        head = head->next;
    }
    return count;
}
// TC: O(N) | SC: O(1)
```

---

# 🟡 MEDIUM PROBLEMS

---

## 26. Add Two Numbers as Linked Lists

**Digits stored in reverse order: `2->4->3` represents 342.**

```cpp
Node* addTwoNumbers(Node* a, Node* b) {
    Node dummy(0);
    Node* tail = &dummy;
    int carry = 0;
    while (a || b || carry) {
        int sum = carry;
        if (a) { sum += a->data; a = a->next; }
        if (b) { sum += b->data; b = b->next; }
        tail->next = new Node(sum % 10);
        tail = tail->next;
        carry = sum / 10;
    }
    return dummy.next;
}
// TC: O(max(N, M)) | SC: O(max(N, M))
```

> **If digits in normal order:** reverse both first, add, reverse result. Or use stacks.

---

## 27. Reverse Linked List in Groups of K

```cpp
Node* reverseKGroup(Node* head, int k) {
    // Check at least k nodes remain
    Node* cur = head;
    for (int i = 0; i < k; i++) {
        if (!cur) return head;
        cur = cur->next;
    }
    // Reverse first k
    Node *prev = nullptr, *node = head;
    for (int i = 0; i < k; i++) {
        Node* next = node->next;
        node->next = prev;
        prev = node;
        node = next;
    }
    head->next = reverseKGroup(node, k);
    return prev;
}
// TC: O(N) | SC: O(N/k) recursion stack
```

---

## 28. Reverse Nodes in Pairs (Swap Pairs)

```cpp
Node* swapPairs(Node* head) {
    Node dummy(0);
    dummy.next = head;
    Node* prev = &dummy;
    while (prev->next && prev->next->next) {
        Node* a = prev->next;
        Node* b = a->next;
        a->next = b->next;
        b->next = a;
        prev->next = b;
        prev = a;
    }
    return dummy.next;
}
// TC: O(N) | SC: O(1)
```

---

## 29. Rotate a Linked List by K

**Right rotation: tail → head, kth from end becomes new head.**

```cpp
Node* rotateRight(Node* head, int k) {
    if (!head || !head->next || k == 0) return head;
    int len = 1;
    Node* tail = head;
    while (tail->next) { tail = tail->next; len++; }
    k = k % len;
    if (k == 0) return head;
    tail->next = head;                 // make circular
    Node* newTail = head;
    for (int i = 1; i < len - k; i++) newTail = newTail->next;
    Node* newHead = newTail->next;
    newTail->next = nullptr;
    return newHead;
}
// TC: O(N) | SC: O(1)
```

---

## 30. Odd-Even Linked List

**Group all nodes at odd positions first, then even — by position, not value.**

```cpp
Node* oddEvenList(Node* head) {
    if (!head || !head->next) return head;
    Node *odd = head, *even = head->next, *evenHead = even;
    while (even && even->next) {
        odd->next = even->next;
        odd = odd->next;
        even->next = odd->next;
        even = even->next;
    }
    odd->next = evenHead;
    return head;
}
// TC: O(N) | SC: O(1)
```

---

## 31. Partition Linked List around X

**All nodes < x come before nodes ≥ x; relative order preserved.**

```cpp
Node* partition(Node* head, int x) {
    Node lessDummy(0), geDummy(0);
    Node *less = &lessDummy, *ge = &geDummy;
    while (head) {
        if (head->data < x) { less->next = head; less = less->next; }
        else                { ge->next = head;   ge = ge->next; }
        head = head->next;
    }
    ge->next = nullptr;
    less->next = geDummy.next;
    return lessDummy.next;
}
// TC: O(N) | SC: O(1)
```

---

## 32. Sort 0s, 1s, 2s Linked List

**Two approaches: count + rewrite, or maintain three lists (like #31).**

```cpp
// Three-list approach (single pass, no overwrite)
Node* sort012(Node* head) {
    Node z(0), o(0), t(0);
    Node *zt = &z, *ot = &o, *tt = &t;
    while (head) {
        if (head->data == 0)      { zt->next = head; zt = zt->next; }
        else if (head->data == 1) { ot->next = head; ot = ot->next; }
        else                      { tt->next = head; tt = tt->next; }
        head = head->next;
    }
    tt->next = nullptr;
    ot->next = t.next;
    zt->next = o.next ? o.next : t.next;
    return z.next ? z.next : (o.next ? o.next : t.next);
}
// TC: O(N) | SC: O(1)
```

---

## 33. Remove Nth Node from End — One Pass

```cpp
Node* removeNthEnd(Node* head, int n) {
    Node dummy(0);
    dummy.next = head;
    Node *fast = &dummy, *slow = &dummy;
    for (int i = 0; i <= n; i++) fast = fast->next;
    while (fast) { fast = fast->next; slow = slow->next; }
    Node* tmp = slow->next;
    slow->next = tmp->next;
    delete tmp;
    return dummy.next;
}
// TC: O(N) | SC: O(1)
```

---

## 34. Reorder List

**L0 → L1 → ... → Ln becomes L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...**

```cpp
void reorderList(Node* head) {
    if (!head || !head->next) return;
    // 1. Find middle
    Node *slow = head, *fast = head;
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    // 2. Reverse second half
    Node* second = reverseIter(slow->next);
    slow->next = nullptr;
    // 3. Merge alternately
    Node *p1 = head, *p2 = second;
    while (p2) {
        Node *t1 = p1->next, *t2 = p2->next;
        p1->next = p2;
        p2->next = t1;
        p1 = t1;
        p2 = t2;
    }
}
// TC: O(N) | SC: O(1)
```

---

## 35. Split a Linked List into K Parts

**Equal-sized parts; earlier parts get the extra node when len isn't divisible.**

```cpp
vector<Node*> splitListToParts(Node* head, int k) {
    int len = length(head);
    int base = len / k, extra = len % k;
    vector<Node*> result(k, nullptr);
    Node* cur = head;
    for (int i = 0; i < k && cur; i++) {
        result[i] = cur;
        int size = base + (i < extra ? 1 : 0);
        for (int j = 0; j < size - 1; j++) cur = cur->next;
        Node* next = cur->next;
        cur->next = nullptr;
        cur = next;
    }
    return result;
}
// TC: O(N) | SC: O(k)
```

---

## 36. Flatten a Multilevel Doubly Linked List

```cpp
struct Node36 {
    int val;
    Node36 *prev, *next, *child;
};

Node36* flatten(Node36* head) {
    if (!head) return head;
    Node36* cur = head;
    stack<Node36*> st;
    while (cur) {
        if (cur->child) {
            if (cur->next) st.push(cur->next);
            cur->next = cur->child;
            cur->child->prev = cur;
            cur->child = nullptr;
        }
        if (!cur->next && !st.empty()) {
            Node36* top = st.top(); st.pop();
            cur->next = top;
            top->prev = cur;
        }
        cur = cur->next;
    }
    return head;
}
// TC: O(N) | SC: O(depth)
```

---

## 37. Clone Linked List with Random Pointer

**O(N) extra space — using HashMap:**

```cpp
RNode* cloneListMap(RNode* head) {
    if (!head) return nullptr;
    unordered_map<RNode*, RNode*> mp;
    for (RNode* cur = head; cur; cur = cur->next) mp[cur] = new RNode(cur->data);
    for (RNode* cur = head; cur; cur = cur->next) {
        mp[cur]->next = mp[cur->next];
        mp[cur]->random = mp[cur->random];
    }
    return mp[head];
}
// TC: O(N) | SC: O(N)
// For O(1) space approach, see #51
```

---

## 38. Swap Kth Node from Beginning and End

```cpp
Node* swapKth(Node* head, int k) {
    int n = length(head);
    if (k > n) return head;
    if (2 * k - 1 == n) return head;          // same node
    Node *prevX = nullptr, *x = head;
    for (int i = 1; i < k; i++) { prevX = x; x = x->next; }
    Node *prevY = nullptr, *y = head;
    for (int i = 1; i < n - k + 1; i++) { prevY = y; y = y->next; }
    if (prevX) prevX->next = y;
    if (prevY) prevY->next = x;
    swap(x->next, y->next);
    if (k == 1) head = y;
    else if (k == n) head = x;
    return head;
}
// TC: O(N) | SC: O(1)
```

---

## 39. Subtract Two Numbers (LL Representation)

**Reverse, subtract digit by digit, handle borrow, reverse result and trim leading zeros.**

```cpp
Node* trimLeadingZeros(Node* head) {
    while (head && head->next && head->data == 0) {
        Node* tmp = head;
        head = head->next;
        delete tmp;
    }
    return head;
}

Node* subtract(Node* a, Node* b) {
    // Assumes a >= b in numerical value
    a = reverseIter(a); b = reverseIter(b);
    Node dummy(0);
    Node* tail = &dummy;
    int borrow = 0;
    while (a) {
        int diff = a->data - borrow - (b ? b->data : 0);
        if (diff < 0) { diff += 10; borrow = 1; }
        else borrow = 0;
        tail->next = new Node(diff);
        tail = tail->next;
        a = a->next;
        if (b) b = b->next;
    }
    Node* result = reverseIter(dummy.next);
    return trimLeadingZeros(result);
}
// TC: O(max(N, M)) | SC: O(max(N, M))
```

---

## 40. Multiply Two Numbers (LL Representation)

```cpp
long long listToNum(Node* head, int mod) {
    long long num = 0;
    while (head) { num = (num * 10 + head->data) % mod; head = head->next; }
    return num;
}

long long multiplyLists(Node* a, Node* b) {
    const int MOD = 1e9 + 7;
    return (listToNum(a, MOD) * listToNum(b, MOD)) % MOD;
}
// TC: O(N + M) | SC: O(1)
// For exact (no mod) result with very large numbers, use string multiplication.
```

---

## 41. Segregate Even and Odd Nodes

**By value — all evens before odds.**

```cpp
Node* segregateEvenOdd(Node* head) {
    Node eDummy(0), oDummy(0);
    Node *e = &eDummy, *o = &oDummy;
    while (head) {
        if (head->data % 2 == 0) { e->next = head; e = e->next; }
        else                     { o->next = head; o = o->next; }
        head = head->next;
    }
    o->next = nullptr;
    e->next = oDummy.next;
    return eDummy.next;
}
// TC: O(N) | SC: O(1)
```

---

## 42. Insert in Sorted Linked List

```cpp
Node* insertSorted(Node* head, int val) {
    Node* node = new Node(val);
    if (!head || head->data >= val) {
        node->next = head;
        return node;
    }
    Node* cur = head;
    while (cur->next && cur->next->data < val) cur = cur->next;
    node->next = cur->next;
    cur->next = node;
    return head;
}
// TC: O(N) | SC: O(1)
```

---

# 🔴 HARD PROBLEMS

---

## 43. Merge K Sorted Lists

**Min-heap approach — O(N log K).**

```cpp
struct Compare {
    bool operator()(Node* a, Node* b) { return a->data > b->data; }
};

Node* mergeKLists(vector<Node*>& lists) {
    priority_queue<Node*, vector<Node*>, Compare> pq;
    for (Node* h : lists) if (h) pq.push(h);
    Node dummy(0);
    Node* tail = &dummy;
    while (!pq.empty()) {
        Node* top = pq.top(); pq.pop();
        tail->next = top;
        tail = tail->next;
        if (top->next) pq.push(top->next);
    }
    return dummy.next;
}
// TC: O(N log K) | SC: O(K)

// Divide & conquer alternative — O(N log K) without heap
Node* mergeRange(vector<Node*>& lists, int l, int r) {
    if (l == r) return lists[l];
    if (l > r)  return nullptr;
    int mid = (l + r) / 2;
    Node* a = mergeRange(lists, l, mid);
    Node* b = mergeRange(lists, mid + 1, r);
    return mergeTwoSorted(a, b);
}
```

---

## 44. Sort a Linked List — Merge Sort O(N log N)

```cpp
Node* getMidForSort(Node* head) {
    Node *slow = head, *fast = head->next;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}

Node* sortList(Node* head) {
    if (!head || !head->next) return head;
    Node* mid = getMidForSort(head);
    Node* right = mid->next;
    mid->next = nullptr;
    return mergeTwoSorted(sortList(head), sortList(right));
}
// TC: O(N log N) | SC: O(log N) recursion
```

---

## 45. Quick Sort on Linked List

```cpp
Node* getTail(Node* head) {
    while (head && head->next) head = head->next;
    return head;
}

Node* partitionLL(Node* head, Node* end, Node** newHead, Node** newEnd) {
    Node* pivot = end;
    Node *prev = nullptr, *cur = head, *tail = pivot;
    while (cur != pivot) {
        if (cur->data < pivot->data) {
            if (!*newHead) *newHead = cur;
            prev = cur;
            cur = cur->next;
        } else {
            if (prev) prev->next = cur->next;
            Node* tmp = cur->next;
            cur->next = nullptr;
            tail->next = cur;
            tail = cur;
            cur = tmp;
        }
    }
    if (!*newHead) *newHead = pivot;
    *newEnd = tail;
    return pivot;
}

Node* quickSortRec(Node* head, Node* end) {
    if (!head || head == end) return head;
    Node *newHead = nullptr, *newEnd = nullptr;
    Node* pivot = partitionLL(head, end, &newHead, &newEnd);
    if (newHead != pivot) {
        Node* tmp = newHead;
        while (tmp->next != pivot) tmp = tmp->next;
        tmp->next = nullptr;
        newHead = quickSortRec(newHead, tmp);
        tmp = getTail(newHead);
        tmp->next = pivot;
    }
    pivot->next = quickSortRec(pivot->next, newEnd);
    return newHead;
}

Node* quickSort(Node* head) {
    return quickSortRec(head, getTail(head));
}
// TC: O(N log N) avg, O(N²) worst | SC: O(log N) recursion
```

---

## 46. Flatten a Linked List (with Bottom Pointers)

**Each node has `next` (right sublist head) and `bottom` (vertical sorted list). Flatten into single sorted bottom-linked list.**

```cpp
struct FNode {
    int data;
    FNode *next, *bottom;
    FNode(int v) : data(v), next(nullptr), bottom(nullptr) {}
};

FNode* mergeBottom(FNode* a, FNode* b) {
    if (!a) return b;
    if (!b) return a;
    FNode* head;
    if (a->data <= b->data) { head = a; a = a->bottom; }
    else                    { head = b; b = b->bottom; }
    FNode* tail = head;
    while (a && b) {
        if (a->data <= b->data) { tail->bottom = a; a = a->bottom; }
        else                    { tail->bottom = b; b = b->bottom; }
        tail = tail->bottom;
    }
    tail->bottom = a ? a : b;
    return head;
}

FNode* flattenLL(FNode* head) {
    if (!head || !head->next) return head;
    head->next = flattenLL(head->next);
    return mergeBottom(head, head->next);
}
// TC: O(N × M) | SC: O(N) recursion stack
```

---

## 47. LRU Cache — DLL + HashMap

```cpp
class LRUCache {
    int cap;
    list<pair<int,int>> dll;          // {key, value}, front = most recent
    unordered_map<int, list<pair<int,int>>::iterator> mp;
public:
    LRUCache(int capacity) : cap(capacity) {}

    int get(int key) {
        if (!mp.count(key)) return -1;
        dll.splice(dll.begin(), dll, mp[key]);
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

## 48. LFU Cache

```cpp
class LFUCache {
    int cap, minFreq;
    unordered_map<int, pair<int,int>> kv;             // key -> {value, freq}
    unordered_map<int, list<int>::iterator> kIter;
    unordered_map<int, list<int>> freqList;
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
            get(key);   // bump frequency
            return;
        }
        if ((int)kv.size() == cap) {
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

## 49. Reverse Doubly Linked List

```cpp
DNode* reverseDLL(DNode* head) {
    DNode *cur = head, *temp = nullptr;
    while (cur) {
        temp = cur->prev;
        cur->prev = cur->next;
        cur->next = temp;
        cur = cur->prev;             // moves to original next
    }
    if (temp) head = temp->prev;     // last cur->prev was new head
    return head;
}
// TC: O(N) | SC: O(1)
```

---

## 50. Detect & Remove Loop in Doubly Linked List

```cpp
void removeLoopDLL(DNode* head) {
    DNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) break;
    }
    if (!fast || !fast->next) return;
    slow = head;
    if (slow == fast) {
        while (fast->next != slow) fast = fast->next;
    } else {
        while (slow->next != fast->next) {
            slow = slow->next;
            fast = fast->next;
        }
    }
    fast->next = nullptr;
    // For DLL, also reset prev of any orphaned starting point if needed
}
// TC: O(N) | SC: O(1)
```

---

## 51. Clone Linked List with Random Pointer — O(1) Extra

**Three-pass: (1) interleave clones, (2) set random, (3) decouple.**

```cpp
RNode* cloneListO1(RNode* head) {
    if (!head) return nullptr;
    // 1. Interleave: A -> A' -> B -> B' -> C -> C'
    for (RNode* cur = head; cur; cur = cur->next->next) {
        RNode* copy = new RNode(cur->data);
        copy->next = cur->next;
        cur->next = copy;
    }
    // 2. Set random pointers on copies
    for (RNode* cur = head; cur; cur = cur->next->next) {
        if (cur->random) cur->next->random = cur->random->next;
    }
    // 3. Decouple
    RNode dummy(0);
    RNode* tail = &dummy;
    for (RNode* cur = head; cur; cur = cur->next) {
        tail->next = cur->next;
        tail = tail->next;
        cur->next = cur->next->next;
    }
    return dummy.next;
}
// TC: O(N) | SC: O(1) — output not counted
```

---

## 52. Convert Binary Tree to Doubly Linked List (Inorder)

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

void btToDllHelper(TreeNode* root, TreeNode*& prev, TreeNode*& head) {
    if (!root) return;
    btToDllHelper(root->left, prev, head);
    if (!prev) head = root;
    else { prev->right = root; root->left = prev; }
    prev = root;
    btToDllHelper(root->right, prev, head);
}

TreeNode* btToDll(TreeNode* root) {
    TreeNode *prev = nullptr, *head = nullptr;
    btToDllHelper(root, prev, head);
    return head;
}
// TC: O(N) | SC: O(H) recursion
```

---

## 53. Convert Sorted Linked List to Balanced BST

```cpp
TreeNode* sortedListToBSTHelper(Node*& head, int n) {
    if (n <= 0) return nullptr;
    TreeNode* left = sortedListToBSTHelper(head, n / 2);
    TreeNode* root = new TreeNode(head->data);
    root->left = left;
    head = head->next;
    root->right = sortedListToBSTHelper(head, n - n / 2 - 1);
    return root;
}

TreeNode* sortedListToBST(Node* head) {
    int n = length(head);
    return sortedListToBSTHelper(head, n);
}
// TC: O(N) | SC: O(log N) recursion (balanced tree)
```

---

## 54. Design Linked List from Scratch

```cpp
class MyLinkedList {
    struct N { int val; N* next; N(int v) : val(v), next(nullptr) {} };
    N* head;
    int sz;
public:
    MyLinkedList() : head(nullptr), sz(0) {}

    int get(int index) {
        if (index < 0 || index >= sz) return -1;
        N* cur = head;
        for (int i = 0; i < index; i++) cur = cur->next;
        return cur->val;
    }

    void addAtHead(int val) {
        N* node = new N(val);
        node->next = head;
        head = node;
        sz++;
    }

    void addAtTail(int val) {
        if (!head) { addAtHead(val); return; }
        N* cur = head;
        while (cur->next) cur = cur->next;
        cur->next = new N(val);
        sz++;
    }

    void addAtIndex(int index, int val) {
        if (index < 0 || index > sz) return;
        if (index == 0) { addAtHead(val); return; }
        N* cur = head;
        for (int i = 0; i < index - 1; i++) cur = cur->next;
        N* node = new N(val);
        node->next = cur->next;
        cur->next = node;
        sz++;
    }

    void deleteAtIndex(int index) {
        if (index < 0 || index >= sz) return;
        if (index == 0) {
            N* tmp = head; head = head->next; delete tmp;
        } else {
            N* cur = head;
            for (int i = 0; i < index - 1; i++) cur = cur->next;
            N* tmp = cur->next; cur->next = tmp->next; delete tmp;
        }
        sz--;
    }
};
// TC: O(N) for index ops, O(1) head | SC: O(N)
```

---

## 55. Skip List Implementation Outline

**Probabilistic structure giving O(log N) avg search/insert/delete with simpler logic than balanced BST.**

```cpp
class SkipList {
    static const int MAX_LEVEL = 16;
    struct Node {
        int val;
        vector<Node*> next;
        Node(int v, int lvl) : val(v), next(lvl, nullptr) {}
    };
    Node* head;
    int curLevel;

    int randomLevel() {
        int lvl = 1;
        while ((rand() & 1) && lvl < MAX_LEVEL) lvl++;
        return lvl;
    }

public:
    SkipList() : curLevel(1) { head = new Node(INT_MIN, MAX_LEVEL); }

    bool search(int target) {
        Node* cur = head;
        for (int i = curLevel - 1; i >= 0; i--)
            while (cur->next[i] && cur->next[i]->val < target)
                cur = cur->next[i];
        cur = cur->next[0];
        return cur && cur->val == target;
    }

    void add(int num) {
        vector<Node*> update(MAX_LEVEL, head);
        Node* cur = head;
        for (int i = curLevel - 1; i >= 0; i--) {
            while (cur->next[i] && cur->next[i]->val < num) cur = cur->next[i];
            update[i] = cur;
        }
        int lvl = randomLevel();
        if (lvl > curLevel) curLevel = lvl;
        Node* node = new Node(num, lvl);
        for (int i = 0; i < lvl; i++) {
            node->next[i] = update[i]->next[i];
            update[i]->next[i] = node;
        }
    }

    bool erase(int num) {
        vector<Node*> update(MAX_LEVEL, nullptr);
        Node* cur = head;
        for (int i = curLevel - 1; i >= 0; i--) {
            while (cur->next[i] && cur->next[i]->val < num) cur = cur->next[i];
            update[i] = cur;
        }
        cur = cur->next[0];
        if (!cur || cur->val != num) return false;
        for (int i = 0; i < curLevel; i++) {
            if (update[i]->next[i] != cur) break;
            update[i]->next[i] = cur->next[i];
        }
        delete cur;
        while (curLevel > 1 && !head->next[curLevel - 1]) curLevel--;
        return true;
    }
};
// TC: O(log N) avg per op | SC: O(N log N) avg
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Pattern Recognition

| Problem Signal | Technique |
|----------------|-----------|
| "Find middle / nth from end" | Slow & fast pointers |
| "Detect cycle / find start" | Floyd's algorithm |
| "Remove duplicates (sorted)" | Single pass, compare with next |
| "Remove duplicates (unsorted)" | HashSet |
| "Merge sorted lists" | Two pointer + dummy head |
| "Reverse list / sublist / pairs / k-group" | Iterative reverse with prev/cur/next |
| "Add / subtract numbers as LL" | Reverse + carry/borrow |
| "Palindrome check" | Half reverse + compare |
| "Reorder / rotate / partition" | Split + manipulate + merge |
| "Random pointer clone" | HashMap or interleaving trick |
| "Sort a list" | Merge sort (O(N log N), O(log N) stack) |
| "Cache (LRU/LFU)" | DLL + HashMap |
| "Y-shape intersection" | Switch heads trick |
| "Multi-level / nested" | Recursion or stack |

---

## ⭐ Top 10 Must-Know

1. **Reverse a Linked List** — three pointers
2. **Slow & Fast Pointers** — middle, cycle, nth from end
3. **Floyd's Cycle Detection** — detect, find start, remove
4. **Dummy Head Trick** — simplifies edge cases for head modifications
5. **Merge Two Sorted Lists** — foundation for merge sort
6. **Reverse in Groups of K** — interview classic
7. **Add Two Numbers** — digit-by-digit with carry
8. **Clone with Random Pointer** — interleaving O(1) trick
9. **LRU Cache** — must-know system design / coding combo
10. **Merge Sort on LL** — O(N log N), O(log N) stack — better than QS

---

## ⭐ Common Pitfalls

✅ **Always check `head == nullptr`** at the start.
✅ **Watch for the tail's next pointer** — set to `nullptr` after rearranging.
✅ **Use a dummy node** when the head can change — avoids special-casing.
✅ **Save next pointer BEFORE modifying current** (`Node* next = cur->next;`).
✅ **Memory leaks** — `delete` removed nodes when freeing them is required.
✅ **Off-by-one in two-pointer gap** — for nth-from-end, advance fast `n` (or `n+1`) steps first.
✅ **Slow/Fast loop condition** — `while (fast && fast->next)` — both required.
✅ **First vs second middle** for even-length lists — pick the right termination.
✅ **`cur->next->next`** access — only after confirming `cur->next` is non-null.
✅ **Recursion stack size** — for very long lists (10⁵+), prefer iterative.

---

## ⭐ Useful STL & Tricks

```cpp
// list = doubly linked list in STL
list<int> ll = {1, 2, 3};
ll.push_back(4);  ll.push_front(0);
ll.pop_back();    ll.pop_front();
ll.splice(ll.begin(), ll, it);   // O(1) move within same list
ll.insert(it, val);              // O(1) insert before iterator
ll.erase(it);                    // O(1) erase

// Forward list = singly linked list
forward_list<int> fl = {1, 2, 3};
fl.push_front(0);
fl.insert_after(fl.begin(), 5);
fl.erase_after(fl.begin());
```

---

## ⭐ Tricks Worth Remembering

| Trick | Use Case |
|-------|----------|
| Slow & fast | Middle / cycle / nth from end |
| `slow = head` after meet | Cycle start (Floyd) |
| Dummy head node | Simplifies head changes |
| Reverse half + compare | Palindrome check O(1) space |
| Switch heads when null | Y-shape intersection |
| Interleave A→A'→B→B'→… | O(1) clone with random pointer |
| Three-list partition | Sort 0/1/2, partition around X |
| List splice for LRU | O(1) move-to-front |
| Reverse → add 1 → reverse | Add 1 to number-as-LL |
| Recursion for backward access | Add/compare LL representing numbers |

---

# 💪 GO ACE THAT TEST!

> **Strategy on the test:**
> 1. **Draw the linked list** on paper before coding — visualize the pointer changes.
> 2. **Use a dummy node** any time the head might change (delete head, merge, reorder).
> 3. **Save `next` before modifying `cur->next`** — losing the rest of the list is the #1 bug.
> 4. **Slow/fast pointers** solve at least half of all LL problems — try first.
> 5. **Test with empty list, 1-node, 2-node, even/odd lengths**.
> 6. **State complexity** — most problems are O(N) time and O(1) space; sort is O(N log N).

**You've got this! 🚀**
