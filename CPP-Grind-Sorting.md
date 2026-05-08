# 🔀 Complete Sorting Algorithms Guide — C++

> **The ultimate sorting sheet — every algorithm, every variant, every interview question.**
> Theory, code, complexity, when to use, dry runs & comparison — all in one place.

---

## 📑 Table of Contents

**FUNDAMENTALS**
- Sorting Terminology
- Complexity Comparison Table
- How to Choose the Right Sort

**COMPARISON-BASED SORTS (1–10)**
1. Bubble Sort
2. Optimized Bubble Sort
3. Selection Sort
4. Insertion Sort
5. Binary Insertion Sort
6. Shell Sort
7. Merge Sort
8. Iterative Merge Sort
9. Quick Sort (Lomuto + Hoare)
10. 3-Way Quick Sort (Dutch National Flag)

**HEAP & TREE BASED (11–13)**
11. Heap Sort
12. Tree Sort (BST-based)
13. Tournament Sort

**NON-COMPARISON SORTS (14–18)**
14. Counting Sort
15. Radix Sort (LSD)
16. Radix Sort (MSD)
17. Bucket Sort
18. Pigeonhole Sort

**HYBRID & SPECIAL (19–25)**
19. Tim Sort (Python / Java built-in)
20. Intro Sort (C++ `std::sort`)
21. Pancake Sort
22. Cycle Sort
23. Cocktail Shaker Sort
24. Gnome Sort
25. Sleep Sort (just for fun)

**EXTERNAL & LARGE-SCALE (26–28)**
26. External Merge Sort
27. K-way Merge with Heap
28. Sorting Linked Lists (Merge Sort variant)

**SORTING PROBLEMS YOU'LL SEE (29–40)**
29. Sort by Custom Comparator
30. Sort by Multiple Keys
31. Stability — Why It Matters
32. Sort 0s, 1s, 2s (Dutch Flag)
33. Wave Sort
34. Sort Nearly Sorted Array (K-sorted)
35. Sort by Frequency
36. Sort Strings by Length / Lexicographically
37. Sort Linked List
38. Find Kth Smallest using QuickSelect
39. Median of an Array
40. Sort an Array of Pairs

**STL SORTING IN C++ (41–45)**
41. `sort`, `stable_sort`, `partial_sort`
42. `nth_element` (Quickselect)
43. Custom Comparators with Lambdas / Functors
44. Sorting Containers (vector, list, map, set)
45. `priority_queue` & Heap Functions

---

## ⚡ Sorting Terminology

| Term | Meaning |
|------|---------|
| **Stable** | Preserves relative order of equal elements |
| **In-place** | Uses O(1) extra space (excluding recursion stack) |
| **Adaptive** | Faster on partially sorted input |
| **Comparison-based** | Compares pairs of elements (Ω(N log N) lower bound) |
| **Non-comparison** | Uses key properties (digits, ranges) — can be O(N) |
| **External** | Data doesn't fit in memory — uses disk |

---

## 📊 Complexity Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable | In-place | Adaptive |
|-----------|------|---------|-------|-------|--------|----------|----------|
| **Bubble** | O(N) | O(N²) | O(N²) | O(1) | ✅ | ✅ | ✅ |
| **Selection** | O(N²) | O(N²) | O(N²) | O(1) | ❌ | ✅ | ❌ |
| **Insertion** | O(N) | O(N²) | O(N²) | O(1) | ✅ | ✅ | ✅ |
| **Shell** | O(N log N) | O(N^1.3) | O(N²) | O(1) | ❌ | ✅ | ✅ |
| **Merge** | O(N log N) | O(N log N) | O(N log N) | O(N) | ✅ | ❌ | ❌ |
| **Quick** | O(N log N) | O(N log N) | O(N²) | O(log N) | ❌ | ✅ | ❌ |
| **3-Way Quick** | O(N) | O(N log N) | O(N²) | O(log N) | ❌ | ✅ | ✅ |
| **Heap** | O(N log N) | O(N log N) | O(N log N) | O(1) | ❌ | ✅ | ❌ |
| **Counting** | O(N+K) | O(N+K) | O(N+K) | O(K) | ✅ | ❌ | ❌ |
| **Radix** | O(D(N+K)) | O(D(N+K)) | O(D(N+K)) | O(N+K) | ✅ | ❌ | ❌ |
| **Bucket** | O(N+K) | O(N+K) | O(N²) | O(N) | ✅ | ❌ | ❌ |
| **Tim** | O(N) | O(N log N) | O(N log N) | O(N) | ✅ | ❌ | ✅ |
| **Intro** | O(N log N) | O(N log N) | O(N log N) | O(log N) | ❌ | ✅ | ❌ |

> **Lower bound for comparison sorts: Ω(N log N)** — proven by decision tree argument.

---

## 🎯 How to Choose the Right Sort

| Situation | Best Choice |
|-----------|-------------|
| General purpose — always | **`std::sort`** (Intro Sort) |
| Need stability | **`std::stable_sort`** (Tim Sort variant) |
| Almost sorted data | **Insertion Sort / Tim Sort** |
| Small N (≤ 10–20) | **Insertion Sort** |
| Integers in known range | **Counting Sort / Radix Sort** |
| Linked list | **Merge Sort** |
| Memory constrained, no stability | **Heap Sort** |
| Need only top K | **Heap (priority_queue)** or `partial_sort` |
| Need only Kth element | **`nth_element`** (Quickselect) |
| Data > RAM | **External Merge Sort** |
| Streaming / Online data | **Heap-based** |

---

# 🟢 COMPARISON-BASED SORTS

---

## 1. Bubble Sort

**Idea:** Repeatedly swap adjacent out-of-order pairs. Largest "bubbles" to the end each pass.

```cpp
#include <bits/stdc++.h>
using namespace std;

void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) swap(arr[j], arr[j + 1]);
        }
    }
}
// TC: O(N²)  | SC: O(1)  | Stable: ✅
```

**Dry run** on `[5, 2, 9, 1]`:
```
Pass 1: [2,5,1,9]   → 9 bubbled to end
Pass 2: [2,1,5,9]
Pass 3: [1,2,5,9]
```

---

## 2. Optimized Bubble Sort (Adaptive)

**Stop early if no swaps in a pass.**

```cpp
void bubbleSortOptimized(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;          // already sorted
    }
}
// Best case: O(N) when already sorted | Stable: ✅
```

---

## 3. Selection Sort

**Idea:** Find min in unsorted portion → swap to front.

```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) minIdx = j;
        }
        if (minIdx != i) swap(arr[i], arr[minIdx]);
    }
}
// TC: O(N²) always | SC: O(1) | Stable: ❌
```

> **Why unstable?** Swapping non-adjacent equal elements can disrupt relative order.
> Useful when **swap cost is high** (selection sort does at most N swaps).

---

## 4. Insertion Sort

**Idea:** Build sorted part one element at a time by inserting into its correct position.

```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i], j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
// TC: Best O(N), Avg/Worst O(N²) | SC: O(1) | Stable: ✅ | Adaptive: ✅
```

> **Best for:** small arrays, nearly sorted data, online algorithm (insert one-at-a-time).
> Used by `std::sort` for small subranges (typically ≤ 16 elements).

---

## 5. Binary Insertion Sort

**Use binary search to find insertion point — fewer comparisons but same shifts.**

```cpp
void binaryInsertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int pos = upper_bound(arr.begin(), arr.begin() + i, key) - arr.begin();
        for (int j = i; j > pos; j--) arr[j] = arr[j - 1];
        arr[pos] = key;
    }
}
// TC: O(N²) shifts dominate | Comparisons: O(N log N) | Stable: ✅
```

---

## 6. Shell Sort

**Idea:** Insertion sort with a gap that shrinks. Original gap sequence by Donald Shell.

```cpp
void shellSort(vector<int>& arr) {
    int n = arr.size();
    for (int gap = n / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < n; i++) {
            int temp = arr[i], j = i;
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            arr[j] = temp;
        }
    }
}
// TC: depends on gap sequence (~O(N log²N) with Shell) | SC: O(1) | Stable: ❌
// Knuth gap sequence (1, 4, 13, 40, ...) tends to perform best.
```

---

## 7. Merge Sort

**Idea:** Divide & conquer — split, recursively sort, merge sorted halves.

```cpp
void merge(vector<int>& arr, int l, int m, int r) {
    vector<int> temp;
    temp.reserve(r - l + 1);
    int i = l, j = m + 1;
    while (i <= m && j <= r) {
        if (arr[i] <= arr[j]) temp.push_back(arr[i++]);
        else                  temp.push_back(arr[j++]);
    }
    while (i <= m) temp.push_back(arr[i++]);
    while (j <= r) temp.push_back(arr[j++]);
    for (int k = 0; k < temp.size(); k++) arr[l + k] = temp[k];
}

void mergeSort(vector<int>& arr, int l, int r) {
    if (l >= r) return;
    int m = l + (r - l) / 2;       // avoid overflow vs (l + r) / 2
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}

void mergeSort(vector<int>& arr) { mergeSort(arr, 0, arr.size() - 1); }
// TC: O(N log N) all cases | SC: O(N) | Stable: ✅
```

> **Best for:** linked lists (no random access penalty), external sorting, when stability is required.

---

## 8. Iterative Merge Sort (Bottom-Up)

**No recursion — useful for languages without good tail-call support, or to avoid stack overflow on huge N.**

```cpp
void mergeSortIterative(vector<int>& arr) {
    int n = arr.size();
    for (int width = 1; width < n; width *= 2) {
        for (int l = 0; l < n; l += 2 * width) {
            int m = min(l + width - 1, n - 1);
            int r = min(l + 2 * width - 1, n - 1);
            if (m < r) merge(arr, l, m, r);
        }
    }
}
// TC: O(N log N) | SC: O(N) | Stable: ✅
```

---

## 9. Quick Sort

**Idea:** Pick pivot → partition (smaller left, greater right) → recurse on partitions.

### Lomuto Partition (simpler, slightly slower)

```cpp
int lomutoPartition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];           // last element as pivot
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int p = lomutoPartition(arr, low, high);
        quickSort(arr, low, p - 1);
        quickSort(arr, p + 1, high);
    }
}
```

### Hoare Partition (faster in practice, original Hoare scheme)

```cpp
int hoarePartition(vector<int>& arr, int low, int high) {
    int pivot = arr[low + (high - low) / 2];
    int i = low - 1, j = high + 1;
    while (true) {
        do { i++; } while (arr[i] < pivot);
        do { j--; } while (arr[j] > pivot);
        if (i >= j) return j;
        swap(arr[i], arr[j]);
    }
}

void quickSortHoare(vector<int>& arr, int low, int high) {
    if (low < high) {
        int p = hoarePartition(arr, low, high);
        quickSortHoare(arr, low, p);
        quickSortHoare(arr, p + 1, high);
    }
}
// TC: Avg O(N log N), Worst O(N²) | SC: O(log N) | Stable: ❌
```

**Pivot strategies (avoid worst case):**
- **Random pivot** — `arr[low + rand() % (high - low + 1)]`
- **Median of three** — pick median of first/middle/last
- **First / last** — vulnerable to sorted input → O(N²) worst case

```cpp
// Randomized quicksort (recommended)
int randomPartition(vector<int>& arr, int low, int high) {
    int randIdx = low + rand() % (high - low + 1);
    swap(arr[randIdx], arr[high]);
    return lomutoPartition(arr, low, high);
}
```

---

## 10. 3-Way Quick Sort (Dutch National Flag)

**Best when there are MANY duplicates — partitions into < pivot, == pivot, > pivot.**

```cpp
void quickSort3Way(vector<int>& arr, int low, int high) {
    if (low >= high) return;
    int lt = low, gt = high;
    int pivot = arr[low];
    int i = low + 1;
    while (i <= gt) {
        if      (arr[i] < pivot) swap(arr[i++], arr[lt++]);
        else if (arr[i] > pivot) swap(arr[i],   arr[gt--]);
        else                     i++;
    }
    quickSort3Way(arr, low, lt - 1);
    quickSort3Way(arr, gt + 1, high);
}
// TC: O(N log N) avg, O(N) when many duplicates | Stable: ❌
```

---

# 🟡 HEAP & TREE BASED

---

## 11. Heap Sort

**Idea:** Build max-heap → repeatedly extract max to end.

```cpp
void heapify(vector<int>& arr, int n, int i) {
    int largest = i, l = 2*i + 1, r = 2*i + 2;
    if (l < n && arr[l] > arr[largest]) largest = l;
    if (r < n && arr[r] > arr[largest]) largest = r;
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();
    // Build max-heap (start from last non-leaf)
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
// TC: O(N log N) all cases | SC: O(1) | Stable: ❌
// Build-heap is O(N), extraction is O(N log N) total.
```

> **Why use it?** Guaranteed O(N log N), in-place. Used as the fallback in Intro Sort when Quick Sort's recursion gets too deep.

---

## 12. Tree Sort (BST-based)

**Insert all elements into BST → in-order traversal yields sorted order.**

```cpp
struct Node { int val; Node *l = nullptr, *r = nullptr; Node(int v) : val(v) {} };

Node* insert(Node* root, int val) {
    if (!root) return new Node(val);
    if (val < root->val) root->l = insert(root->l, val);
    else                 root->r = insert(root->r, val);
    return root;
}

void inorder(Node* root, vector<int>& out) {
    if (!root) return;
    inorder(root->l, out);
    out.push_back(root->val);
    inorder(root->r, out);
}

void treeSort(vector<int>& arr) {
    Node* root = nullptr;
    for (int x : arr) root = insert(root, x);
    arr.clear();
    inorder(root, arr);
    // (memory cleanup omitted for brevity — use unique_ptr in real code)
}
// TC: Avg O(N log N), Worst O(N²) (unbalanced) | SC: O(N)
// Balanced BST (AVL/RB) → guaranteed O(N log N)
```

---

## 13. Tournament Sort

**Build "tournament tree" — winner advances, replace winner, run another round.** Useful for k-way merge / external sorting. Conceptually similar to a heap.

```cpp
// Sketch — replace winner with sentinel, propagate
// Practical implementations use std::priority_queue.
```

---

# 🔴 NON-COMPARISON SORTS

---

## 14. Counting Sort

**Idea:** Count occurrences of each key. Works when keys lie in a small range.

```cpp
void countingSort(vector<int>& arr) {
    if (arr.empty()) return;
    int mn = *min_element(arr.begin(), arr.end());
    int mx = *max_element(arr.begin(), arr.end());
    int range = mx - mn + 1;

    vector<int> count(range, 0);
    for (int x : arr) count[x - mn]++;

    // Prefix sum (for stability)
    for (int i = 1; i < range; i++) count[i] += count[i - 1];

    vector<int> output(arr.size());
    // Iterate from right for stability
    for (int i = arr.size() - 1; i >= 0; i--) {
        output[--count[arr[i] - mn]] = arr[i];
    }
    arr = output;
}
// TC: O(N + K) | SC: O(N + K) | Stable: ✅
// Where K = mx - mn + 1
```

> **Key insight:** beats O(N log N) when K = O(N). Useless if range is huge (e.g., 32-bit integers).

---

## 15. Radix Sort (LSD — Least Significant Digit)

**Idea:** Sort by digit, starting from least significant. Use stable sort (counting) per digit.

```cpp
void countingSortByDigit(vector<int>& arr, int exp) {
    int n = arr.size();
    vector<int> output(n);
    int count[10] = {0};

    for (int x : arr) count[(x / exp) % 10]++;
    for (int i = 1; i < 10; i++) count[i] += count[i - 1];

    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[--count[digit]] = arr[i];
    }
    arr = output;
}

void radixSortLSD(vector<int>& arr) {
    if (arr.empty()) return;
    int mx = *max_element(arr.begin(), arr.end());
    for (int exp = 1; mx / exp > 0; exp *= 10)
        countingSortByDigit(arr, exp);
}
// TC: O(D × (N + K)) where D = digits, K = base (10) | SC: O(N + K) | Stable: ✅
// Doesn't work for negatives directly — split into negatives/positives first.
```

---

## 16. Radix Sort (MSD — Most Significant Digit)

**Recursive — sort by leading digit, then bucket and recurse.** Better for strings of varying length.

```cpp
// Sketch — used for sorting strings, IP addresses, etc.
// Each recursive call buckets by next digit/character.
```

---

## 17. Bucket Sort

**Idea:** Distribute into buckets by key range, sort each bucket, concatenate.**

```cpp
void bucketSort(vector<float>& arr) {        // for floats in [0, 1)
    int n = arr.size();
    vector<vector<float>> buckets(n);

    for (float x : arr) {
        int idx = min((int)(x * n), n - 1);
        buckets[idx].push_back(x);
    }

    for (auto& b : buckets) sort(b.begin(), b.end());

    int idx = 0;
    for (auto& b : buckets)
        for (float x : b) arr[idx++] = x;
}
// TC: Avg O(N + K), Worst O(N²) if all in one bucket | SC: O(N + K) | Stable: ✅ (if inner sort is)
// Best for uniformly distributed data.
```

---

## 18. Pigeonhole Sort

**Like counting sort — for small ranges of integers.**

```cpp
void pigeonholeSort(vector<int>& arr) {
    if (arr.empty()) return;
    int mn = *min_element(arr.begin(), arr.end());
    int mx = *max_element(arr.begin(), arr.end());
    int range = mx - mn + 1;

    vector<vector<int>> holes(range);
    for (int x : arr) holes[x - mn].push_back(x);

    int idx = 0;
    for (auto& h : holes)
        for (int x : h) arr[idx++] = x;
}
// TC: O(N + range) | SC: O(N + range) | Stable: ✅
```

---

# 🟣 HYBRID & SPECIAL

---

## 19. Tim Sort

**Hybrid of merge sort + insertion sort. Default in Python (`sorted`, `list.sort`) and Java (`Arrays.sort` for objects).**

**Idea:**
1. Find natural "runs" (already-sorted chunks)
2. Extend short runs to MIN_RUN (typically 32) using insertion sort
3. Merge runs using a stack with size invariants

```cpp
const int RUN = 32;

void insertionSortRange(vector<int>& arr, int l, int r) {
    for (int i = l + 1; i <= r; i++) {
        int key = arr[i], j = i - 1;
        while (j >= l && arr[j] > key) { arr[j+1] = arr[j]; j--; }
        arr[j+1] = key;
    }
}

void timSortSimplified(vector<int>& arr) {
    int n = arr.size();
    // Sort small chunks with insertion sort
    for (int i = 0; i < n; i += RUN)
        insertionSortRange(arr, i, min(i + RUN - 1, n - 1));
    // Merge runs
    for (int size = RUN; size < n; size *= 2) {
        for (int l = 0; l < n; l += 2 * size) {
            int m = min(l + size - 1, n - 1);
            int r = min(l + 2 * size - 1, n - 1);
            if (m < r) merge(arr, l, m, r);
        }
    }
}
// TC: Best O(N), Avg/Worst O(N log N) | SC: O(N) | Stable: ✅
```

> Real Tim Sort is much more sophisticated (run detection, galloping merge, etc.). Above is a simplified version.

---

## 20. Intro Sort

**Default for `std::sort` in C++.**
- Starts with **quicksort**
- Switches to **heapsort** when recursion depth exceeds 2 × log₂(N) (avoids O(N²))
- Falls back to **insertion sort** for small subarrays (≤ 16)

```cpp
void introSortHelper(vector<int>& arr, int low, int high, int depthLimit) {
    if (high - low + 1 < 16) {
        insertionSortRange(arr, low, high);
        return;
    }
    if (depthLimit == 0) {
        // Heap sort the [low, high] range
        make_heap(arr.begin() + low, arr.begin() + high + 1);
        sort_heap(arr.begin() + low, arr.begin() + high + 1);
        return;
    }
    int p = lomutoPartition(arr, low, high);
    introSortHelper(arr, low, p - 1, depthLimit - 1);
    introSortHelper(arr, p + 1, high, depthLimit - 1);
}

void introSort(vector<int>& arr) {
    int n = arr.size();
    int depthLimit = 2 * (int)log2(n);
    introSortHelper(arr, 0, n - 1, depthLimit);
}
// TC: O(N log N) guaranteed | SC: O(log N) | Stable: ❌
```

> **Why the genius?** Quicksort's average performance + heapsort's worst-case guarantee + insertion sort's small-N speed.

---

## 21. Pancake Sort

**Sort by repeatedly "flipping" prefix.** Only allowed operation is reverse(0..k). Used in real interviews and bioinformatics.

```cpp
void flip(vector<int>& arr, int k) {
    reverse(arr.begin(), arr.begin() + k + 1);
}

void pancakeSort(vector<int>& arr) {
    int n = arr.size();
    for (int curSize = n; curSize > 1; curSize--) {
        int maxIdx = max_element(arr.begin(), arr.begin() + curSize) - arr.begin();
        if (maxIdx != curSize - 1) {
            if (maxIdx != 0) flip(arr, maxIdx);     // bring max to front
            flip(arr, curSize - 1);                 // move to its final position
        }
    }
}
// TC: O(N²) | SC: O(1) | Stable: ❌
```

---

## 22. Cycle Sort

**Minimum number of WRITES — useful when writes are expensive (Flash/EEPROM).**

```cpp
void cycleSort(vector<int>& arr) {
    int n = arr.size();
    for (int cycleStart = 0; cycleStart < n - 1; cycleStart++) {
        int item = arr[cycleStart];
        int pos = cycleStart;
        for (int i = cycleStart + 1; i < n; i++)
            if (arr[i] < item) pos++;
        if (pos == cycleStart) continue;
        while (item == arr[pos]) pos++;
        swap(item, arr[pos]);
        while (pos != cycleStart) {
            pos = cycleStart;
            for (int i = cycleStart + 1; i < n; i++)
                if (arr[i] < item) pos++;
            while (item == arr[pos]) pos++;
            swap(item, arr[pos]);
        }
    }
}
// TC: O(N²) | SC: O(1) | Writes: O(N) | Stable: ❌
```

---

## 23. Cocktail Shaker Sort

**Bidirectional bubble sort — bubble largest right, then smallest left.**

```cpp
void cocktailSort(vector<int>& arr) {
    int n = arr.size();
    bool swapped = true;
    int start = 0, end = n - 1;
    while (swapped) {
        swapped = false;
        for (int i = start; i < end; i++) {
            if (arr[i] > arr[i + 1]) { swap(arr[i], arr[i + 1]); swapped = true; }
        }
        if (!swapped) break;
        swapped = false;
        end--;
        for (int i = end - 1; i >= start; i--) {
            if (arr[i] > arr[i + 1]) { swap(arr[i], arr[i + 1]); swapped = true; }
        }
        start++;
    }
}
// TC: O(N²) | SC: O(1) | Stable: ✅
```

---

## 24. Gnome Sort

**Like sorting flowerpots — if current pair OK, move forward; else swap & step back.** Simplest possible.

```cpp
void gnomeSort(vector<int>& arr) {
    int i = 0, n = arr.size();
    while (i < n) {
        if (i == 0 || arr[i] >= arr[i - 1]) i++;
        else { swap(arr[i], arr[i - 1]); i--; }
    }
}
// TC: O(N²) | SC: O(1) | Stable: ✅
```

---

## 25. Sleep Sort (just for fun!)

**For positive integers — spawn a thread for each element that sleeps that many ms, then prints.**

```cpp
// Joke algorithm — DON'T use in production!
// Each thread sleeps `value` ms then appends to result.
// Smaller values "wake up" first → sorted output (sort of).
// Unreliable, OS-dependent, but classic interview lore.
```

---

# 💾 EXTERNAL & LARGE-SCALE SORTING

---

## 26. External Merge Sort

**Sort data that doesn't fit in RAM.**

```
Step 1 (Run formation):
  Read chunk that fits in RAM → sort → write to disk as "run"
  Repeat until all data is in sorted runs.

Step 2 (Merge phase):
  Open k runs simultaneously.
  Use min-heap to pick smallest from k fronts.
  Write to output. Refill from same run.

If k runs > memory, do multi-pass merge (k-way each pass).
```

```cpp
// Sketch
void externalSort(string inFile, string outFile, int memSize) {
    // 1. Split into sorted runs
    vector<string> runs;
    ifstream in(inFile);
    while (in) {
        vector<int> chunk;
        // read up to memSize ints
        int x;
        for (int i = 0; i < memSize && in >> x; i++) chunk.push_back(x);
        sort(chunk.begin(), chunk.end());
        string runFile = "run_" + to_string(runs.size());
        ofstream out(runFile);
        for (int v : chunk) out << v << "\n";
        runs.push_back(runFile);
    }
    // 2. K-way merge runs into outFile (using priority_queue)
    // ... see #27
}
```

---

## 27. K-way Merge with Heap

**Merge K sorted lists/streams in O(N log K).**

```cpp
struct Item {
    int val, listIdx, elemIdx;
    bool operator>(const Item& o) const { return val > o.val; }
};

vector<int> kWayMerge(vector<vector<int>>& lists) {
    priority_queue<Item, vector<Item>, greater<Item>> pq;
    for (int i = 0; i < lists.size(); i++)
        if (!lists[i].empty()) pq.push({lists[i][0], i, 0});

    vector<int> result;
    while (!pq.empty()) {
        Item top = pq.top(); pq.pop();
        result.push_back(top.val);
        if (top.elemIdx + 1 < lists[top.listIdx].size())
            pq.push({lists[top.listIdx][top.elemIdx + 1], top.listIdx, top.elemIdx + 1});
    }
    return result;
}
// TC: O(N log K) | SC: O(K)
```

---

## 28. Sorting Linked Lists (Merge Sort)

**Merge sort is ideal for linked lists — no random access penalty, O(1) extra (excluding stack).**

```cpp
struct Node { int val; Node* next; Node(int v) : val(v), next(nullptr) {} };

Node* getMid(Node* head) {
    Node *slow = head, *fast = head->next;
    while (fast && fast->next) { slow = slow->next; fast = fast->next->next; }
    return slow;
}

Node* mergeLists(Node* a, Node* b) {
    Node dummy(0); Node* tail = &dummy;
    while (a && b) {
        if (a->val <= b->val) { tail->next = a; a = a->next; }
        else                  { tail->next = b; b = b->next; }
        tail = tail->next;
    }
    tail->next = a ? a : b;
    return dummy.next;
}

Node* mergeSortLL(Node* head) {
    if (!head || !head->next) return head;
    Node* mid = getMid(head);
    Node* right = mid->next;
    mid->next = nullptr;
    return mergeLists(mergeSortLL(head), mergeSortLL(right));
}
// TC: O(N log N) | SC: O(log N) recursion | Stable: ✅
```

---

# 🎯 SORTING PROBLEMS YOU'LL SEE

---

## 29. Sort by Custom Comparator

```cpp
// Ascending: default (operator<)
sort(arr.begin(), arr.end());

// Descending
sort(arr.begin(), arr.end(), greater<int>());

// Lambda
sort(arr.begin(), arr.end(), [](int a, int b) { return a > b; });

// Sort points by distance from origin
sort(pts.begin(), pts.end(), [](auto& a, auto& b) {
    return a.x*a.x + a.y*a.y < b.x*b.x + b.y*b.y;
});

// CRITICAL: comparator MUST be strict-weak-ordering.
// Use < (not <=) — equal returns false.
```

---

## 30. Sort by Multiple Keys

```cpp
struct Person { string name; int age; double salary; };

// By age asc, then salary desc, then name
sort(people.begin(), people.end(), [](const Person& a, const Person& b) {
    if (a.age    != b.age)    return a.age    <  b.age;
    if (a.salary != b.salary) return a.salary >  b.salary;
    return a.name < b.name;
});

// Or with tuple — concise!
sort(people.begin(), people.end(), [](const auto& a, const auto& b) {
    return tie(a.age, b.salary, a.name) < tie(b.age, a.salary, b.name);
    //         ^^ careful with desc — flip operands explicitly
});
```

---

## 31. Stability — Why It Matters

**Stable sort keeps relative order of equal elements.**

```cpp
// Items already sorted by name.
// Now sort by city — within same city, names should still be sorted.
stable_sort(items.begin(), items.end(),
            [](auto& a, auto& b) { return a.city < b.city; });
```

> **`std::sort` is NOT stable. `std::stable_sort` is.**

---

## 32. Sort 0s, 1s, 2s — Dutch National Flag

```cpp
void sortColors(vector<int>& arr) {
    int low = 0, mid = 0, high = arr.size() - 1;
    while (mid <= high) {
        if      (arr[mid] == 0) swap(arr[low++], arr[mid++]);
        else if (arr[mid] == 1) mid++;
        else                    swap(arr[mid], arr[high--]);
    }
}
// TC: O(N) one pass | SC: O(1)
```

---

## 33. Wave Sort

**Result: arr[0] ≥ arr[1] ≤ arr[2] ≥ arr[3] …**

```cpp
void waveSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n; i += 2) {
        if (i > 0     && arr[i] < arr[i - 1]) swap(arr[i], arr[i - 1]);
        if (i < n - 1 && arr[i] < arr[i + 1]) swap(arr[i], arr[i + 1]);
    }
}
// TC: O(N) | SC: O(1)
```

---

## 34. Sort Nearly-Sorted (K-Sorted) Array

**Each element is at most K positions away from its sorted position. Use min-heap of size K+1.**

```cpp
void kSort(vector<int>& arr, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;
    for (int i = 0; i <= k && i < arr.size(); i++) pq.push(arr[i]);

    int idx = 0;
    for (int i = k + 1; i < arr.size(); i++) {
        arr[idx++] = pq.top(); pq.pop();
        pq.push(arr[i]);
    }
    while (!pq.empty()) { arr[idx++] = pq.top(); pq.pop(); }
}
// TC: O(N log K) | SC: O(K)
```

---

## 35. Sort by Frequency

```cpp
vector<int> sortByFrequency(vector<int>& arr) {
    unordered_map<int, int> freq;
    for (int x : arr) freq[x]++;

    sort(arr.begin(), arr.end(), [&](int a, int b) {
        if (freq[a] != freq[b]) return freq[a] > freq[b];   // higher freq first
        return a < b;                                        // tie-break by value
    });
    return arr;
}
// TC: O(N log N) | SC: O(N)
```

---

## 36. Sort Strings

```cpp
// Lexicographically (default)
sort(words.begin(), words.end());

// By length, then lexicographically
sort(words.begin(), words.end(), [](const string& a, const string& b) {
    if (a.size() != b.size()) return a.size() < b.size();
    return a < b;
});

// Case-insensitive
sort(words.begin(), words.end(), [](const string& a, const string& b) {
    return lexicographical_compare(a.begin(), a.end(), b.begin(), b.end(),
                                   [](char x, char y) { return tolower(x) < tolower(y); });
});

// Largest concatenated number — sort by "a+b > b+a"
string largestNumber(vector<int>& nums) {
    vector<string> s;
    for (int n : nums) s.push_back(to_string(n));
    sort(s.begin(), s.end(), [](auto& a, auto& b) { return a + b > b + a; });
    if (s[0] == "0") return "0";
    string res; for (auto& x : s) res += x;
    return res;
}
```

---

## 37. Sort Linked List

See `#28` — merge sort is the standard answer. **Why not quicksort?**
- Linked list has no random access → median pivot finding is O(N).
- Merge sort works equally well on linked lists, with O(1) actual extra space.

---

## 38. Find Kth Smallest — Quickselect

**Like quicksort but only recurse into the side containing position K.**

```cpp
int partition(vector<int>& arr, int l, int r) {
    int pivot = arr[r], i = l - 1;
    for (int j = l; j < r; j++)
        if (arr[j] <= pivot) swap(arr[++i], arr[j]);
    swap(arr[i+1], arr[r]);
    return i + 1;
}

int quickSelect(vector<int>& arr, int l, int r, int k) {
    if (l == r) return arr[l];
    int p = partition(arr, l, r);
    if (p == k)     return arr[p];
    else if (k < p) return quickSelect(arr, l, p - 1, k);
    else            return quickSelect(arr, p + 1, r, k);
}

int kthSmallest(vector<int> arr, int k) {
    return quickSelect(arr, 0, arr.size() - 1, k - 1);
}
// TC: Avg O(N), Worst O(N²) | SC: O(1)
// std::nth_element does this in C++ STL!
```

---

## 39. Median of an Array

```cpp
double findMedian(vector<int>& arr) {
    int n = arr.size();
    nth_element(arr.begin(), arr.begin() + n/2, arr.end());
    if (n % 2 == 1) return arr[n/2];
    int upper = arr[n/2];
    nth_element(arr.begin(), arr.begin() + n/2 - 1, arr.begin() + n/2);
    return (upper + arr[n/2 - 1]) / 2.0;
}
// TC: O(N) avg using nth_element
```

---

## 40. Sort an Array of Pairs

```cpp
vector<pair<int, string>> v = {{3, "a"}, {1, "b"}, {3, "c"}};

// Default — sorts by first, then second
sort(v.begin(), v.end());

// By second only
sort(v.begin(), v.end(), [](auto& a, auto& b) { return a.second < b.second; });

// By first desc, second asc
sort(v.begin(), v.end(), [](auto& a, auto& b) {
    if (a.first != b.first) return a.first > b.first;
    return a.second < b.second;
});
```

---

# 🛠️ STL SORTING IN C++

---

## 41. `sort`, `stable_sort`, `partial_sort`

```cpp
sort(v.begin(), v.end());              // Intro Sort, O(N log N), unstable
stable_sort(v.begin(), v.end());       // Tim Sort variant, O(N log N), stable

// Partial sort: smallest K elements at front, sorted
partial_sort(v.begin(), v.begin() + 5, v.end());
// First 5 are smallest, sorted. Rest in unspecified order. O(N log K)

// partial_sort_copy — copies smallest K into another container
vector<int> top5(5);
partial_sort_copy(big.begin(), big.end(), top5.begin(), top5.end());

// is_sorted — check
if (is_sorted(v.begin(), v.end())) ...;
auto it = is_sorted_until(v.begin(), v.end());  // first violator
```

---

## 42. `nth_element` — Quickselect

```cpp
// Place k-th smallest at position k.
// Elements before are ≤ it; after are ≥ it. NOT FULLY SORTED.
nth_element(v.begin(), v.begin() + k, v.end());
int kthSmallest = v[k];

// With custom comparator
nth_element(v.begin(), v.begin() + k, v.end(), greater<int>());
// Now v[k] is the k-th LARGEST.
// TC: O(N) average — much faster than full sort if you only need one element.
```

---

## 43. Custom Comparators — Lambdas / Functors

```cpp
// Lambda (most common)
sort(v.begin(), v.end(), [](int a, int b) { return a > b; });

// Capture
int threshold = 100;
sort(v.begin(), v.end(), [threshold](int a, int b) {
    return abs(a - threshold) < abs(b - threshold);
});

// Functor (use when reusing comparator)
struct ByAbs {
    bool operator()(int a, int b) const { return abs(a) < abs(b); }
};
sort(v.begin(), v.end(), ByAbs());

// Function pointer
bool desc(int a, int b) { return a > b; }
sort(v.begin(), v.end(), desc);
```

> **CRITICAL:** comparator must be **strict weak ordering**. Means: `cmp(x, x) == false`. Use `<` not `<=`. Crashes / undefined behavior otherwise.

---

## 44. Sorting Containers

```cpp
// vector — use std::sort
sort(v.begin(), v.end());

// array — same
array<int, 5> a = {3, 1, 2};
sort(a.begin(), a.end());

// list (doubly linked) — has its own sort
list<int> l = {3, 1, 2};
l.sort();
l.sort(greater<int>());

// forward_list — has its own sort too
forward_list<int> fl = {3, 1, 2};
fl.sort();

// set / map are ALREADY sorted (red-black tree)
// — sorted by key automatically; cannot std::sort them.

// To sort a map by VALUE, copy to vector first:
vector<pair<string, int>> v(myMap.begin(), myMap.end());
sort(v.begin(), v.end(), [](auto& a, auto& b) { return a.second < b.second; });

// To sort an unordered_map similarly:
// Same — copy to vector first; unordered containers are not sortable.
```

---

## 45. `priority_queue` & Heap Functions

```cpp
// priority_queue = max-heap by default
priority_queue<int> pq;
pq.push(3); pq.push(1); pq.push(5);
pq.top();           // 5
pq.pop();

// Min-heap
priority_queue<int, vector<int>, greater<int>> minHeap;

// Custom comparator
auto cmp = [](pair<int,int>& a, pair<int,int>& b) {
    return a.second > b.second;        // min-heap by second
};
priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq2(cmp);

// Heap functions on vector (lower-level than priority_queue)
vector<int> v = {3, 1, 5, 2, 4};
make_heap(v.begin(), v.end());        // builds max-heap in-place, O(N)
pop_heap(v.begin(), v.end()); v.pop_back();   // remove max
v.push_back(7); push_heap(v.begin(), v.end()); // add element

sort_heap(v.begin(), v.end());        // O(N log N), once heaped
is_heap(v.begin(), v.end());

// Top K largest using heap (size K, push/pop)
priority_queue<int, vector<int>, greater<int>> minH;
for (int x : nums) {
    minH.push(x);
    if (minH.size() > k) minH.pop();
}
// TC: O(N log K) — way better than O(N log N) full sort if K << N.
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Common Interview Questions

> **Q: What's the fastest sorting algorithm?**
> A: It depends on input. For general-purpose comparison sorting, average O(N log N) is the best you can do. **Quicksort** is fastest in practice on average; **mergesort** has guaranteed O(N log N); **non-comparison sorts** (counting/radix) can do O(N) when constraints allow.

> **Q: What's the time complexity lower bound?**
> A: Comparison-based sorts have a **Ω(N log N) lower bound**, proven via decision trees (N! permutations need log₂(N!) ≈ N log N comparisons). Non-comparison sorts can beat this.

> **Q: Why is stable sort important?**
> A: Preserves relative order of equal elements. Crucial when sorting by multiple keys sequentially (e.g., sort by city, then keep names within city alphabetical).

> **Q: Why is mergesort preferred for linked lists?**
> A: Linked lists lack random access. Quicksort needs a fast median pivot, which is O(N). Mergesort's split is O(N) (find middle), and merge is naturally efficient. Plus mergesort is O(1) extra space on linked lists (vs O(N) on arrays).

> **Q: When does quicksort go O(N²)?**
> A: When pivot is always the smallest or largest (worst case). Sorted/reverse-sorted input with first/last pivot triggers it. Fix: **randomized pivot**, **median-of-three**, or use **introsort**.

> **Q: What does C++ `std::sort` use?**
> A: **Introsort** — quicksort, switching to heapsort when recursion depth > 2 log₂ N, and insertion sort for small ranges. O(N log N) guaranteed, fast in practice.

> **Q: What does Python's `sorted()` use?**
> A: **Tim Sort** — hybrid of mergesort + insertion sort. Stable. O(N) on already-sorted, O(N log N) worst case. Java also uses it for Object arrays.

> **Q: When use heapsort over quicksort?**
> A: When you need **guaranteed O(N log N)** worst case AND O(1) space. Quicksort is faster on random data; heapsort wins on adversarial input.

> **Q: When use counting/radix sort?**
> A: When keys are integers in a **small known range** (counting) or have **few digits** (radix). Beats O(N log N) but uses extra space.

> **Q: How do you sort more data than fits in RAM?**
> A: **External merge sort** — sort chunks that fit, write as runs, then K-way merge using a heap.

> **Q: Quicksort vs mergesort — pros/cons?**
> A: Quicksort: in-place, fast in practice, cache-friendly, but O(N²) worst case, unstable. Mergesort: stable, guaranteed O(N log N), good for linked lists/external, but O(N) extra space.

> **Q: What's `nth_element`?**
> A: STL's quickselect — O(N) average. Places the k-th smallest at position k; doesn't fully sort. Perfect when you only need top K or median.

> **Q: How do you find the median efficiently?**
> A: `nth_element` for O(N) average. Or two heaps (max-heap of lower half, min-heap of upper half) for streaming data.

> **Q: How would you implement a stable quicksort?**
> A: Standard quicksort isn't stable. To make it stable, attach original indices to elements, use them as tiebreakers in compares, then drop after sort. Costs extra memory; usually use mergesort instead.

> **Q: What's the difference between `partial_sort` and `nth_element`?**
> A: `partial_sort` puts the K smallest at the front, **sorted**. `nth_element` only ensures the K-th element is in place; faster but the K elements before are unsorted.

---

## ⭐ Common Pitfalls

✅ **Comparator must be strict weak ordering** — use `<`, not `<=`. Otherwise undefined behavior.
✅ **`std::sort` is not stable.** Use `std::stable_sort` if equal-element order matters.
✅ **Quicksort worst case** — always randomize pivot or use median-of-three.
✅ **Counting sort with huge range** = wasted memory. Only good for small K.
✅ **Recursion stack overflow** — for huge arrays, prefer iterative merge sort or `std::sort`.
✅ **Comparator integer overflow** — `return a - b;` overflows for INT_MIN/INT_MAX. Use `return a < b;`.
✅ **Mutating elements during sort** — undefined behavior.
✅ **Sorting `std::list`** — use `list::sort()`, not `std::sort` (which needs random access).
✅ **`nth_element` doesn't fully sort** — only positions k-th element.
✅ **Heap doesn't return things sorted** — only top is correct. Pop repeatedly to get sorted order.

---

## ⭐ Decision Tree for Sorting

```
Need to sort something?
│
├─ Need stability?
│  └─ Yes → std::stable_sort (or mergesort)
│
├─ Only need top/bottom K?
│  └─ Use heap of size K, or partial_sort
│
├─ Only need k-th element?
│  └─ Use nth_element (O(N) average)
│
├─ Integers in small range (≤ N)?
│  └─ Counting sort or radix sort
│
├─ Linked list?
│  └─ Mergesort
│
├─ Almost sorted / very small N?
│  └─ Insertion sort
│
├─ Data > RAM?
│  └─ External merge sort
│
└─ Default → std::sort (Introsort)
```

---

## ⭐ Real-World Sort Selection

| Language | Algorithm Used |
|----------|----------------|
| **C++** `std::sort` | Introsort (quick + heap + insertion) |
| **C++** `std::stable_sort` | Mergesort variant |
| **Java** `Arrays.sort(int[])` | Dual-pivot quicksort |
| **Java** `Arrays.sort(Object[])` | TimSort |
| **Python** `sorted` / `list.sort` | TimSort |
| **JavaScript** `Array.prototype.sort` | TimSort (V8) — was insertion sort + quicksort pre-2018 |
| **Rust** `slice.sort` | TimSort variant |
| **Go** `sort.Sort` | Pattern-defeating quicksort (pdqsort) |

---

## ⭐ Top 10 Things to Remember

1. **`std::sort`** is your default — Introsort, O(N log N) guaranteed.
2. **`std::stable_sort`** when equal-element order matters.
3. **`nth_element`** for K-th element (O(N) avg).
4. **`partial_sort`** for top-K sorted at front.
5. **Heap (priority_queue)** for streaming top-K.
6. **Quicksort** randomize pivot — never use first/last on sorted data.
7. **Mergesort** is the linked-list answer.
8. **Counting/Radix** beats O(N log N) when keys are small integers.
9. **Comparator must be strict weak ordering** (`<`, not `<=`).
10. **Lower bound for comparison sorts is Ω(N log N)** — proven, can't beat.

---

## ⭐ Practice Problems Worth Solving

| Problem | What You'll Learn |
|---------|-------------------|
| LeetCode 912 (Sort an Array) | Implement multiple algorithms |
| LeetCode 75 (Sort Colors) | Dutch National Flag |
| LeetCode 215 (Kth Largest) | Heap / Quickselect |
| LeetCode 148 (Sort List) | Mergesort on linked list |
| LeetCode 179 (Largest Number) | Custom comparator on strings |
| LeetCode 274 (H-Index) | Counting sort |
| LeetCode 1122 (Relative Sort Array) | Custom comparator with map |
| LeetCode 295 (Median Stream) | Two heaps |
| LeetCode 23 (Merge K Sorted Lists) | K-way merge with heap |
| LeetCode 252 (Meeting Rooms) | Sort by start time |
| LeetCode 56 (Merge Intervals) | Sort + merge |
| LeetCode 977 (Squares of Sorted Array) | Two pointer |
| LeetCode 451 (Sort by Frequency) | Bucket / heap |
| LeetCode 692 (Top K Frequent Words) | Custom comparator + heap |

---

# 💪 GO ACE THAT SORTING QUESTION!

> **Test-day strategy:**
> 1. **Default to `std::sort`** unless told otherwise — interviewer usually wants to see knowledge of complexity.
> 2. Be ready to **implement merge sort, quick sort, heap sort** by hand.
> 3. Know **when each non-comparison sort applies** (ranges, digits, distribution).
> 4. **Comparator pitfalls** — strict weak ordering, no overflow.
> 5. **State complexity clearly** — best, average, worst, space, stable.
> 6. **Discuss tradeoffs** — interviewer often asks "why this over that?"
> 7. For huge data: **external merge sort + K-way merge with heap**.
>
> **You've got this! 🚀**
