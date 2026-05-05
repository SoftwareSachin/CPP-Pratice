# 🔢 Complete Math Problems Interview Sheet — C++

> **The ultimate math problems guide for coding interviews & tests.**
> Factorial, Prime, GCD/LCM, Power, Digits, Number Theory, Modular Arithmetic & more.

---

## 📑 Table of Contents

**BASIC (1–15)**
1. Factorial of a Number (Iterative + Recursive)
2. Sum of First N Natural Numbers
3. Sum of Squares / Cubes of First N
4. Count Digits in a Number
5. Reverse a Number
6. Check Palindrome Number
7. Check Armstrong Number
8. Sum of Digits
9. Product of Digits
10. Print All Divisors of N
11. Sum of All Divisors
12. Check Prime Number
13. Power of a Number (Iterative)
14. Power Using Recursion (Fast Power)
15. Last Digit of N^K

**INTERMEDIATE (16–35)**
16. GCD / HCF (Euclidean Algorithm)
17. LCM of Two Numbers
18. GCD of Array
19. LCM of Array
20. Check Perfect Number
21. Check Strong Number
22. Check Automorphic Number
23. Check Krishnamurthy Number
24. Check Happy Number
25. Check Harshad Number
26. Sieve of Eratosthenes (All Primes up to N)
27. Count Primes up to N
28. Smallest / Largest Prime Factor
29. Prime Factorization
30. Number of Trailing Zeros in N!
31. Fibonacci Number (Iterative + Matrix)
32. Catalan Numbers
33. nCr / Binomial Coefficient
34. Pascal's Triangle Row
35. Tower of Hanoi (Move Count)

**ADVANCED (36–55)**
36. Modular Exponentiation (a^b % m)
37. Modular Inverse (Fermat's Little Theorem)
38. nCr % p (Lucas Theorem / Precomputation)
39. Extended Euclidean Algorithm
40. Linear Diophantine Equations
41. Chinese Remainder Theorem
42. Euler's Totient Function
43. Segmented Sieve
44. Number of Divisors of N
45. Sum of Divisors of N
46. Pow(x, n) — Including Negative Exponents
47. Sqrt(x) Using Binary Search
48. Cube Root Using Binary Search
49. Nth Root of M
50. Integer to Roman / Roman to Integer
51. Excel Column Title to Number & vice versa
52. Add / Multiply Two Large Numbers (as strings)
53. Josephus Problem
54. Greatest Common Divisor of Strings
55. Ugly Numbers / Super Ugly Numbers

---

## ⚡ Quick Complexity Reference

| Operation | Time | Notes |
|-----------|------|-------|
| Factorial | O(N) | Use `long long` / BigInt |
| GCD (Euclidean) | O(log min(a,b)) | Fast |
| Prime check | O(√N) | Or O(1) after sieve |
| Sieve of Eratosthenes | O(N log log N) | All primes ≤ N |
| Fast power | O(log N) | Modular too |
| Count digits | O(log₁₀ N) | Or `(int)log10(N)+1` |
| Prime factorization | O(√N) | Trial division |
| Modular inverse | O(log p) | Fermat (p prime) |

---

# 🟢 BASIC PROBLEMS

---

## 1. Factorial of a Number

**Problem:** Compute N! = 1 × 2 × 3 × ... × N.

```cpp
#include <bits/stdc++.h>
using namespace std;

// Iterative
long long factorial(int n) {
    long long fact = 1;
    for (int i = 2; i <= n; i++) fact *= i;
    return fact;
}

// Recursive
long long factorialRec(int n) {
    if (n <= 1) return 1;
    return n * factorialRec(n - 1);
}
// TC: O(N) | SC: O(1) iterative, O(N) recursive (stack)
```

> **Note:** 21! overflows `long long`. For larger N, use BigInt (string-based multiplication, see #52).

---

## 2. Sum of First N Natural Numbers

```cpp
// Formula approach — O(1)
long long sumN(int n) {
    return (long long)n * (n + 1) / 2;
}

// Loop approach — O(N)
long long sumNLoop(int n) {
    long long sum = 0;
    for (int i = 1; i <= n; i++) sum += i;
    return sum;
}
```

---

## 3. Sum of Squares / Cubes of First N

```cpp
// 1² + 2² + ... + N² = N(N+1)(2N+1) / 6
long long sumOfSquares(int n) {
    return (long long)n * (n + 1) * (2 * n + 1) / 6;
}

// 1³ + 2³ + ... + N³ = (N(N+1)/2)²
long long sumOfCubes(int n) {
    long long s = (long long)n * (n + 1) / 2;
    return s * s;
}
// TC: O(1) | SC: O(1)
```

---

## 4. Count Digits in a Number

```cpp
int countDigits(int n) {
    if (n == 0) return 1;
    int count = 0;
    n = abs(n);
    while (n > 0) {
        count++;
        n /= 10;
    }
    return count;
}

// Using log10 — O(1)
int countDigitsLog(int n) {
    if (n == 0) return 1;
    return (int)log10(abs(n)) + 1;
}
// TC: O(log₁₀ N) loop | O(1) log
```

---

## 5. Reverse a Number

```cpp
int reverseNumber(int n) {
    int rev = 0;
    while (n > 0) {
        int digit = n % 10;
        // Overflow check (LeetCode style)
        if (rev > (INT_MAX - digit) / 10) return 0;
        rev = rev * 10 + digit;
        n /= 10;
    }
    return rev;
}
// TC: O(log₁₀ N) | SC: O(1)
```

---

## 6. Check Palindrome Number

```cpp
bool isPalindrome(int n) {
    if (n < 0) return false;
    int original = n, rev = 0;
    while (n > 0) {
        rev = rev * 10 + n % 10;
        n /= 10;
    }
    return rev == original;
}
// TC: O(log₁₀ N) | SC: O(1)
```

---

## 7. Check Armstrong Number

**A number equals the sum of its digits raised to the power of digit count.**
Example: 153 = 1³ + 5³ + 3³

```cpp
bool isArmstrong(int n) {
    int original = n;
    int digits = (int)log10(n) + 1;
    int sum = 0;
    while (n > 0) {
        int d = n % 10;
        sum += pow(d, digits);
        n /= 10;
    }
    return sum == original;
}
// TC: O(log₁₀ N) | SC: O(1)
```

---

## 8. Sum of Digits

```cpp
int sumOfDigits(int n) {
    int sum = 0;
    n = abs(n);
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}
// TC: O(log₁₀ N) | SC: O(1)
```

---

## 9. Product of Digits

```cpp
int productOfDigits(int n) {
    int product = 1;
    n = abs(n);
    if (n == 0) return 0;
    while (n > 0) {
        product *= n % 10;
        n /= 10;
    }
    return product;
}
// TC: O(log₁₀ N) | SC: O(1)
```

---

## 10. Print All Divisors of N

```cpp
vector<int> allDivisors(int n) {
    vector<int> divisors;
    for (int i = 1; (long long)i * i <= n; i++) {
        if (n % i == 0) {
            divisors.push_back(i);
            if (i != n / i) divisors.push_back(n / i);
        }
    }
    sort(divisors.begin(), divisors.end());
    return divisors;
}
// TC: O(√N log √N) | SC: O(√N)
```

---

## 11. Sum of All Divisors

```cpp
// Sum of divisors of single number
int sumDivisors(int n) {
    int sum = 0;
    for (int i = 1; (long long)i * i <= n; i++) {
        if (n % i == 0) {
            sum += i;
            if (i != n / i) sum += n / i;
        }
    }
    return sum;
}

// Sum of divisors of all numbers 1 to N (efficient)
long long sumDivisorsAll(int n) {
    long long sum = 0;
    for (int i = 1; i <= n; i++) sum += (long long)(n / i) * i;
    return sum;
}
// TC: O(N) for "all" version
```

---

## 12. Check Prime Number

```cpp
bool isPrime(int n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    // Check 6k ± 1 up to √n
    for (int i = 5; (long long)i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) return false;
    }
    return true;
}
// TC: O(√N) | SC: O(1)
```

---

## 13. Power of a Number (Iterative)

```cpp
long long power(long long base, int exp) {
    long long result = 1;
    while (exp > 0) {
        if (exp & 1) result *= base;
        base *= base;
        exp >>= 1;
    }
    return result;
}
// TC: O(log exp) | SC: O(1) — Binary exponentiation
```

---

## 14. Power Using Recursion (Fast Power)

```cpp
double myPow(double x, long long n) {
    if (n == 0) return 1.0;
    if (n < 0) { x = 1 / x; n = -n; }
    double half = myPow(x, n / 2);
    return (n & 1) ? half * half * x : half * half;
}
// TC: O(log N) | SC: O(log N) recursion stack
```

---

## 15. Last Digit of N^K

```cpp
int lastDigit(int n, int k) {
    // Cycle of last digits repeats every 4
    if (k == 0) return 1;
    int last = n % 10;
    int cycleIdx = (k - 1) % 4;
    int result = 1;
    for (int i = 0; i <= cycleIdx; i++) result = (result * last) % 10;
    return result;
}

// Using fast power with mod 10
int lastDigitFast(long long n, long long k) {
    if (k == 0) return 1;
    long long base = n % 10, ans = 1;
    while (k > 0) {
        if (k & 1) ans = (ans * base) % 10;
        base = (base * base) % 10;
        k >>= 1;
    }
    return (int)ans;
}
// TC: O(log K) | SC: O(1)
```

---

# 🟡 INTERMEDIATE PROBLEMS

---

## 16. GCD / HCF — Euclidean Algorithm

```cpp
// Recursive
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}

// Iterative
int gcdIter(int a, int b) {
    while (b) {
        a %= b;
        swap(a, b);
    }
    return a;
}
// TC: O(log min(a, b)) | SC: O(1) iterative
```

> **C++17:** use `__gcd(a, b)` or `std::gcd(a, b)` from `<numeric>`.

---

## 17. LCM of Two Numbers

```cpp
long long lcm(int a, int b) {
    return (long long)a / gcd(a, b) * b;  // divide first to avoid overflow
}
// TC: O(log min(a, b))
```

---

## 18. GCD of Array

```cpp
int gcdArray(vector<int>& arr) {
    int result = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        result = gcd(result, arr[i]);
        if (result == 1) return 1;
    }
    return result;
}
// TC: O(N log(max))
```

---

## 19. LCM of Array

```cpp
long long lcmArray(vector<int>& arr) {
    long long result = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        result = result / __gcd(result, (long long)arr[i]) * arr[i];
    }
    return result;
}
// TC: O(N log(max))
```

---

## 20. Check Perfect Number

**Sum of proper divisors equals the number itself.** (e.g. 6 = 1+2+3, 28 = 1+2+4+7+14)

```cpp
bool isPerfect(int n) {
    if (n <= 1) return false;
    int sum = 1;  // 1 is a proper divisor
    for (int i = 2; (long long)i * i <= n; i++) {
        if (n % i == 0) {
            sum += i;
            if (i != n / i) sum += n / i;
        }
    }
    return sum == n;
}
// TC: O(√N) | SC: O(1)
```

---

## 21. Check Strong Number

**A number is strong if sum of factorial of its digits equals the number.** (e.g., 145 = 1! + 4! + 5!)

```cpp
int factOfDigit(int d) {
    int f = 1;
    for (int i = 2; i <= d; i++) f *= i;
    return f;
}

bool isStrong(int n) {
    int original = n, sum = 0;
    while (n > 0) {
        sum += factOfDigit(n % 10);
        n /= 10;
    }
    return sum == original;
}
// TC: O(log₁₀ N)
```

---

## 22. Check Automorphic Number

**A number whose square ends with the number itself.** (e.g., 5² = 25, 25² = 625, 76² = 5776)

```cpp
bool isAutomorphic(int n) {
    long long sq = (long long)n * n;
    while (n > 0) {
        if (n % 10 != sq % 10) return false;
        n /= 10;
        sq /= 10;
    }
    return true;
}
// TC: O(log₁₀ N) | SC: O(1)
```

---

## 23. Check Krishnamurthy Number

**Same as strong number — sum of factorial of digits = number.** (Synonym for #21)

```cpp
bool isKrishnamurthy(int n) {
    return isStrong(n);
}
```

---

## 24. Check Happy Number

**Replace number with sum of squares of digits; if it eventually reaches 1, it's happy.**

```cpp
int sumSquaredDigits(int n) {
    int sum = 0;
    while (n > 0) {
        int d = n % 10;
        sum += d * d;
        n /= 10;
    }
    return sum;
}

bool isHappy(int n) {
    // Floyd's cycle detection
    int slow = n, fast = n;
    do {
        slow = sumSquaredDigits(slow);
        fast = sumSquaredDigits(sumSquaredDigits(fast));
    } while (slow != fast);
    return slow == 1;
}
// TC: O(log N) | SC: O(1)
```

---

## 25. Check Harshad (Niven) Number

**A number divisible by the sum of its digits.** (e.g., 18 → 1+8=9, 18%9=0)

```cpp
bool isHarshad(int n) {
    int sum = 0, original = n;
    while (n > 0) { sum += n % 10; n /= 10; }
    return original % sum == 0;
}
// TC: O(log₁₀ N)
```

---

## 26. Sieve of Eratosthenes — All Primes up to N

```cpp
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; (long long)i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
        }
    }
    return isPrime;
}

vector<int> getPrimes(int n) {
    vector<bool> p = sieve(n);
    vector<int> primes;
    for (int i = 2; i <= n; i++)
        if (p[i]) primes.push_back(i);
    return primes;
}
// TC: O(N log log N) | SC: O(N)
```

---

## 27. Count Primes up to N

```cpp
int countPrimes(int n) {
    if (n < 2) return 0;
    vector<bool> isPrime(n, true);
    isPrime[0] = isPrime[1] = false;
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (isPrime[i]) {
            count++;
            for (long long j = (long long)i * i; j < n; j += i)
                isPrime[j] = false;
        }
    }
    return count;
}
// TC: O(N log log N)
```

---

## 28. Smallest / Largest Prime Factor

```cpp
int smallestPrimeFactor(int n) {
    if (n % 2 == 0) return 2;
    for (int i = 3; (long long)i * i <= n; i += 2)
        if (n % i == 0) return i;
    return n;  // n itself is prime
}

int largestPrimeFactor(int n) {
    int largest = -1;
    while (n % 2 == 0) { largest = 2; n /= 2; }
    for (int i = 3; (long long)i * i <= n; i += 2) {
        while (n % i == 0) { largest = i; n /= i; }
    }
    if (n > 2) largest = n;
    return largest;
}
// TC: O(√N) | SC: O(1)
```

---

## 29. Prime Factorization

```cpp
vector<int> primeFactors(int n) {
    vector<int> factors;
    while (n % 2 == 0) { factors.push_back(2); n /= 2; }
    for (int i = 3; (long long)i * i <= n; i += 2) {
        while (n % i == 0) { factors.push_back(i); n /= i; }
    }
    if (n > 2) factors.push_back(n);
    return factors;
}

// As map of (prime → exponent)
map<int, int> primeFactorization(int n) {
    map<int, int> mp;
    while (n % 2 == 0) { mp[2]++; n /= 2; }
    for (int i = 3; (long long)i * i <= n; i += 2) {
        while (n % i == 0) { mp[i]++; n /= i; }
    }
    if (n > 2) mp[n]++;
    return mp;
}
// TC: O(√N)
```

---

## 30. Number of Trailing Zeros in N!

**Count factors of 5 in N!** (factors of 2 are always more abundant).

```cpp
int trailingZerosFact(int n) {
    int count = 0;
    while (n > 0) {
        n /= 5;
        count += n;
    }
    return count;
}
// TC: O(log₅ N) | SC: O(1)
// 100! has 100/5 + 100/25 = 20 + 4 = 24 trailing zeros
```

---

## 31. Fibonacci Number

```cpp
// Iterative — O(N)
long long fib(int n) {
    if (n < 2) return n;
    long long a = 0, b = 1;
    for (int i = 2; i <= n; i++) {
        long long c = a + b;
        a = b;
        b = c;
    }
    return b;
}

// Matrix exponentiation — O(log N)
typedef vector<vector<long long>> Matrix;
Matrix multiply(Matrix A, Matrix B) {
    Matrix C(2, vector<long long>(2, 0));
    for (int i = 0; i < 2; i++)
        for (int j = 0; j < 2; j++)
            for (int k = 0; k < 2; k++)
                C[i][j] += A[i][k] * B[k][j];
    return C;
}

Matrix matPower(Matrix M, long long p) {
    Matrix result = {{1, 0}, {0, 1}};  // identity
    while (p > 0) {
        if (p & 1) result = multiply(result, M);
        M = multiply(M, M);
        p >>= 1;
    }
    return result;
}

long long fibFast(long long n) {
    if (n == 0) return 0;
    Matrix base = {{1, 1}, {1, 0}};
    Matrix res = matPower(base, n);
    return res[0][1];
}
// TC: O(log N) for matrix
```

---

## 32. Catalan Numbers

**C(n) = (2n)! / ((n+1)! × n!)** — counts balanced parentheses, BSTs, paths, etc.

```cpp
long long catalan(int n) {
    vector<long long> C(n + 1, 0);
    C[0] = C[1] = 1;
    for (int i = 2; i <= n; i++)
        for (int j = 0; j < i; j++)
            C[i] += C[j] * C[i - 1 - j];
    return C[n];
}
// TC: O(N²) | SC: O(N)

// Closed form using nCr
long long catalanFormula(int n) {
    long long c = 1;
    for (int i = 0; i < n; i++) {
        c = c * (2 * n - i) / (i + 1);
    }
    return c / (n + 1);
}
// TC: O(N)
```

---

## 33. nCr / Binomial Coefficient

```cpp
// Iterative — avoids factorial overflow
long long nCr(int n, int r) {
    if (r > n - r) r = n - r;  // C(n, r) = C(n, n-r)
    long long result = 1;
    for (int i = 0; i < r; i++) {
        result *= (n - i);
        result /= (i + 1);
    }
    return result;
}
// TC: O(R) | SC: O(1)

// Pascal's table — useful for many queries
vector<vector<long long>> buildPascal(int n) {
    vector<vector<long long>> C(n + 1, vector<long long>(n + 1, 0));
    for (int i = 0; i <= n; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++)
            C[i][j] = C[i-1][j-1] + C[i-1][j];
    }
    return C;
}
// TC: O(N²) | SC: O(N²)
```

---

## 34. Pascal's Triangle Row

```cpp
vector<long long> pascalRow(int row) {
    vector<long long> ans = {1};
    long long val = 1;
    for (int i = 1; i < row; i++) {
        val = val * (row - i) / i;
        ans.push_back(val);
    }
    return ans;
}
// TC: O(row) | SC: O(row)
```

---

## 35. Tower of Hanoi — Move Count

**Minimum moves to transfer N disks = 2^N − 1.**

```cpp
long long hanoiMoves(int n) {
    return (1LL << n) - 1;
}

// Print actual moves
void hanoi(int n, char from, char to, char aux) {
    if (n == 0) return;
    hanoi(n - 1, from, aux, to);
    cout << "Move disk " << n << " from " << from << " to " << to << "\n";
    hanoi(n - 1, aux, to, from);
}
// TC: O(2^N) for printing
```

---

# 🔴 ADVANCED PROBLEMS

---

## 36. Modular Exponentiation — a^b % m

```cpp
long long modPow(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = result * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}
// TC: O(log exp) | SC: O(1)
```

---

## 37. Modular Inverse — Fermat's Little Theorem

**Valid only when `m` is prime: `a⁻¹ ≡ a^(m-2) (mod m)`.**

```cpp
long long modInverse(long long a, long long m) {
    return modPow(a, m - 2, m);
}
// TC: O(log m)
```

For non-prime `m`, use Extended Euclidean (#39).

---

## 38. nCr % p

**Precompute factorials and inverse factorials when p is prime.**

```cpp
const int MOD = 1e9 + 7;
const int MAXN = 1e6 + 5;
long long fact[MAXN], invFact[MAXN];

void precompute() {
    fact[0] = 1;
    for (int i = 1; i < MAXN; i++) fact[i] = fact[i - 1] * i % MOD;
    invFact[MAXN - 1] = modPow(fact[MAXN - 1], MOD - 2, MOD);
    for (int i = MAXN - 2; i >= 0; i--)
        invFact[i] = invFact[i + 1] * (i + 1) % MOD;
}

long long nCrMod(int n, int r) {
    if (r < 0 || r > n) return 0;
    return fact[n] * invFact[r] % MOD * invFact[n - r] % MOD;
}
// TC: O(1) per query after O(N) precomputation
```

---

## 39. Extended Euclidean Algorithm

**Find x, y such that a·x + b·y = gcd(a, b).**

```cpp
long long extGcd(long long a, long long b, long long &x, long long &y) {
    if (b == 0) { x = 1; y = 0; return a; }
    long long x1, y1;
    long long g = extGcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - (a / b) * y1;
    return g;
}

// Modular inverse for any (a, m) with gcd(a, m) = 1
long long modInverseExt(long long a, long long m) {
    long long x, y;
    long long g = extGcd(a, m, x, y);
    if (g != 1) return -1;  // no inverse exists
    return (x % m + m) % m;
}
// TC: O(log min(a, b))
```

---

## 40. Linear Diophantine Equations

**Solve a·x + b·y = c. Solution exists iff gcd(a, b) divides c.**

```cpp
bool solveDiophantine(long long a, long long b, long long c,
                      long long &x, long long &y) {
    long long g = extGcd(abs(a), abs(b), x, y);
    if (c % g != 0) return false;
    x *= c / g; y *= c / g;
    if (a < 0) x = -x;
    if (b < 0) y = -y;
    return true;
}
// General solution: (x + k·b/g, y - k·a/g) for any integer k
```

---

## 41. Chinese Remainder Theorem (CRT)

**Given system: x ≡ rᵢ (mod mᵢ) where mᵢ are pairwise coprime, find x.**

```cpp
long long CRT(vector<long long>& r, vector<long long>& m) {
    long long M = 1;
    for (long long mi : m) M *= mi;
    long long x = 0;
    for (int i = 0; i < r.size(); i++) {
        long long Mi = M / m[i];
        long long invMi = modInverseExt(Mi, m[i]);
        x = (x + r[i] * Mi % M * invMi) % M;
    }
    return (x + M) % M;
}
// TC: O(K log M) where K is number of equations
```

---

## 42. Euler's Totient Function

**φ(n) = count of integers in [1, n] coprime to n.**

```cpp
int phi(int n) {
    int result = n;
    for (int p = 2; (long long)p * p <= n; p++) {
        if (n % p == 0) {
            while (n % p == 0) n /= p;
            result -= result / p;
        }
    }
    if (n > 1) result -= result / n;
    return result;
}
// TC: O(√N)

// Sieve-based — phi for all 1..N
vector<int> phiSieve(int n) {
    vector<int> phi(n + 1);
    iota(phi.begin(), phi.end(), 0);
    for (int i = 2; i <= n; i++) {
        if (phi[i] == i) {  // i is prime
            for (int j = i; j <= n; j += i)
                phi[j] -= phi[j] / i;
        }
    }
    return phi;
}
// TC: O(N log log N)
```

---

## 43. Segmented Sieve — Primes in [L, R]

```cpp
vector<int> segmentedSieve(long long L, long long R) {
    long long lim = sqrt(R) + 1;
    vector<bool> mark(lim + 1, false);
    vector<int> primes;
    for (long long i = 2; i <= lim; i++) {
        if (!mark[i]) {
            primes.push_back((int)i);
            for (long long j = i * i; j <= lim; j += i) mark[j] = true;
        }
    }
    vector<bool> isPrime(R - L + 1, true);
    for (int p : primes) {
        long long start = max((long long)p * p, ((L + p - 1) / p) * p);
        for (long long j = start; j <= R; j += p) isPrime[j - L] = false;
    }
    if (L == 1) isPrime[0] = false;
    vector<int> result;
    for (long long i = L; i <= R; i++)
        if (isPrime[i - L]) result.push_back((int)i);
    return result;
}
// TC: O((R - L + 1) log log √R + √R log log √R)
```

---

## 44. Number of Divisors of N

**If N = p₁^a · p₂^b · ... then d(N) = (a+1)(b+1)...**

```cpp
int numDivisors(int n) {
    int count = 1;
    for (int p = 2; (long long)p * p <= n; p++) {
        if (n % p == 0) {
            int exp = 0;
            while (n % p == 0) { n /= p; exp++; }
            count *= (exp + 1);
        }
    }
    if (n > 1) count *= 2;
    return count;
}
// TC: O(√N)
```

---

## 45. Sum of Divisors of N

**σ(N) = Π ((p^(a+1) − 1) / (p − 1)) over prime factorization.**

```cpp
long long sumOfDivisors(int n) {
    long long sum = 1;
    for (int p = 2; (long long)p * p <= n; p++) {
        if (n % p == 0) {
            long long term = 1, current = 1;
            while (n % p == 0) {
                current *= p;
                term += current;
                n /= p;
            }
            sum *= term;
        }
    }
    if (n > 1) sum *= (1 + n);
    return sum;
}
// TC: O(√N)
```

---

## 46. Pow(x, n) — Including Negative Exponents

```cpp
double myPow(double x, int n) {
    long long N = n;
    if (N < 0) { x = 1 / x; N = -N; }
    double result = 1.0;
    while (N > 0) {
        if (N & 1) result *= x;
        x *= x;
        N >>= 1;
    }
    return result;
}
// TC: O(log |N|) | SC: O(1)
```

---

## 47. Sqrt(x) Using Binary Search

```cpp
int mySqrt(int x) {
    if (x < 2) return x;
    long long low = 1, high = x / 2, ans = 0;
    while (low <= high) {
        long long mid = (low + high) / 2;
        if (mid * mid <= x) { ans = mid; low = mid + 1; }
        else high = mid - 1;
    }
    return (int)ans;
}
// TC: O(log X) | SC: O(1)
```

---

## 48. Cube Root Using Binary Search

```cpp
int cubeRoot(int x) {
    long long low = 0, high = 1e6, ans = 0;
    while (low <= high) {
        long long mid = (low + high) / 2;
        long long cube = mid * mid * mid;
        if (cube == x) return (int)mid;
        if (cube < x) { ans = mid; low = mid + 1; }
        else high = mid - 1;
    }
    return (int)ans;  // floor of cube root
}
// TC: O(log X)
```

---

## 49. Nth Root of M

```cpp
long long power(long long base, int n, long long m) {
    long long ans = 1;
    for (int i = 0; i < n; i++) {
        ans *= base;
        if (ans > m) return m + 1;  // overflow guard
    }
    return ans;
}

int nthRoot(int n, int m) {
    int low = 1, high = m;
    while (low <= high) {
        int mid = (low + high) / 2;
        long long val = power(mid, n, m);
        if (val == m) return mid;
        if (val < m) low = mid + 1;
        else high = mid - 1;
    }
    return -1;  // no integer nth root
}
// TC: O(log M × N) | SC: O(1)
```

---

## 50. Integer to Roman / Roman to Integer

```cpp
string intToRoman(int num) {
    vector<pair<int, string>> v = {
        {1000, "M"}, {900, "CM"}, {500, "D"}, {400, "CD"},
        {100, "C"},  {90, "XC"},  {50, "L"},  {40, "XL"},
        {10, "X"},   {9, "IX"},   {5, "V"},   {4, "IV"}, {1, "I"}
    };
    string res;
    for (auto& [val, sym] : v) {
        while (num >= val) { res += sym; num -= val; }
    }
    return res;
}

int romanToInt(string s) {
    unordered_map<char, int> mp = {
        {'I',1},{'V',5},{'X',10},{'L',50},
        {'C',100},{'D',500},{'M',1000}
    };
    int sum = 0;
    for (int i = 0; i < s.size(); i++) {
        if (i + 1 < s.size() && mp[s[i]] < mp[s[i+1]]) sum -= mp[s[i]];
        else sum += mp[s[i]];
    }
    return sum;
}
// TC: O(N)
```

---

## 51. Excel Column Title ↔ Number

```cpp
// Number → Title (1 → "A", 28 → "AB")
string toTitle(int n) {
    string res;
    while (n > 0) {
        n--;
        res = char('A' + n % 26) + res;
        n /= 26;
    }
    return res;
}

// Title → Number ("A" → 1, "AB" → 28)
int titleToNumber(string s) {
    int result = 0;
    for (char c : s) result = result * 26 + (c - 'A' + 1);
    return result;
}
// TC: O(L) where L is title length
```

---

## 52. Add / Multiply Two Large Numbers (as Strings)

```cpp
// Add two non-negative number strings
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

// Multiply two non-negative number strings
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
// TC: O(N + M) for add, O(N × M) for multiply
```

---

## 53. Josephus Problem

**N people in circle, every k-th eliminated. Find survivor index (0-based).**

```cpp
// Recursive
int josephus(int n, int k) {
    if (n == 1) return 0;
    return (josephus(n - 1, k) + k) % n;
}

// Iterative — O(N), O(1) space
int josephusIter(int n, int k) {
    int result = 0;
    for (int i = 2; i <= n; i++) result = (result + k) % i;
    return result;
}
// TC: O(N) | SC: O(1) iterative
```

---

## 54. Greatest Common Divisor of Strings

**Find largest string `x` such that `x` concatenated repeatedly forms both `s1` and `s2`.**

```cpp
string gcdOfStrings(string s1, string s2) {
    if (s1 + s2 != s2 + s1) return "";
    int g = __gcd((int)s1.size(), (int)s2.size());
    return s1.substr(0, g);
}
// TC: O(N + M) | SC: O(N + M)
```

---

## 55. Ugly Numbers / Super Ugly Numbers

**Ugly number — only prime factors 2, 3, 5.** (1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, ...)

```cpp
// nth ugly number — DP / 3-pointer
int nthUgly(int n) {
    vector<int> ugly(n);
    ugly[0] = 1;
    int i2 = 0, i3 = 0, i5 = 0;
    for (int i = 1; i < n; i++) {
        int next = min({ugly[i2] * 2, ugly[i3] * 3, ugly[i5] * 5});
        ugly[i] = next;
        if (next == ugly[i2] * 2) i2++;
        if (next == ugly[i3] * 3) i3++;
        if (next == ugly[i5] * 5) i5++;
    }
    return ugly[n - 1];
}
// TC: O(N) | SC: O(N)

// Super Ugly — primes given as input array
int nthSuperUgly(int n, vector<int>& primes) {
    int k = primes.size();
    vector<long long> ugly(n);
    vector<int> idx(k, 0);
    ugly[0] = 1;
    for (int i = 1; i < n; i++) {
        long long next = LLONG_MAX;
        for (int j = 0; j < k; j++)
            next = min(next, ugly[idx[j]] * primes[j]);
        ugly[i] = next;
        for (int j = 0; j < k; j++)
            if (next == ugly[idx[j]] * primes[j]) idx[j]++;
    }
    return (int)ugly[n - 1];
}
// TC: O(N × K)
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Common Math Identities

| Identity | Formula |
|----------|---------|
| Sum of 1 to N | N(N+1)/2 |
| Sum of squares | N(N+1)(2N+1)/6 |
| Sum of cubes | (N(N+1)/2)² |
| Sum of AP | N/2 × (2a + (N−1)d) |
| Sum of GP | a(rⁿ−1)/(r−1) |
| Number of digits | ⌊log₁₀ N⌋ + 1 |
| Trailing zeros in N! | N/5 + N/25 + N/125 + ... |
| GCD × LCM | a × b |
| nCr = nCn-r | symmetry |
| nCr = n−1Cr−1 + n−1Cr | Pascal's identity |
| Fermat's Little | a^(p−1) ≡ 1 (mod p) when gcd(a,p)=1 |
| Wilson's | (p−1)! ≡ −1 (mod p) iff p prime |

---

## ⭐ Pattern Recognition

| Problem Signal | Technique |
|----------------|-----------|
| "Find a^b mod m" | Modular exponentiation |
| "Count primes / check prime" | Sieve / 6k±1 trial division |
| "GCD / LCM" | Euclidean algorithm |
| "Sum / count of divisors" | Prime factorization formulas |
| "Number repeats digit operation" | Cycle detection / Floyd |
| "nCr with large N" | Precompute factorials + modular inverse |
| "Find x in modular equation" | Extended Euclidean / CRT |
| "Number with property of digits" | Digit extraction + check |
| "Linear recurrence" | Matrix exponentiation |
| "Floor of √N / ∛N" | Binary search |

---

## ⭐ Top 10 Must-Know

1. **Sieve of Eratosthenes** — bulk primality
2. **Euclidean GCD** — and LCM via gcd
3. **Binary Exponentiation** — fast power
4. **Modular Exponentiation** — `a^b % m`
5. **Modular Inverse** — Fermat / Extended Euclidean
6. **Prime Factorization** — O(√N) trial
7. **Trailing zeros in N!** — count factors of 5
8. **Fast nCr** — precomputed factorials mod p
9. **Sqrt via binary search** — pattern for any monotonic function
10. **Digit DP basics** — extract digits, sum, reverse, palindrome

---

## ⭐ Common Pitfalls

✅ **Overflow:** Use `long long` for products of two ints near 10⁹
✅ **Division before multiplication** in `lcm`: `a / gcd(a,b) * b` (not `a * b / gcd`)
✅ **Edge cases:** N=0, N=1, negative inputs
✅ **`(int)log10(0)` is undefined** — guard with `if (n == 0) return 1;`
✅ **`pow()` returns double** — risky for integers, prefer custom `power()`
✅ **Modular operations:** always `((x % m) + m) % m` for negatives
✅ **Floating-point sqrt:** prefer integer binary search for exact answers
✅ **Recursive factorial / fib:** stack overflow for large N — use iterative
✅ **Empty product = 1, empty sum = 0** — initial values matter
✅ **Mod must be prime** for Fermat's inverse — otherwise use Extended Euclidean

---

## ⭐ Useful STL Snippets

```cpp
__gcd(a, b);                 // built-in GCD (also std::gcd in <numeric>)
std::lcm(a, b);              // C++17, in <numeric>
abs(x);                      // for int / long long
fabs(x);                     // for double
pow(a, b);                   // double, careful!
sqrt(x); cbrt(x);            // double
log10(x); log2(x); log(x);   // natural log
ceil(x); floor(x); round(x); // double
ceil((double)a / b);         // integer ceiling division
(a + b - 1) / b;             // integer ceil without floats (a, b > 0)
```

---

# 💪 GO ACE THAT TEST!

> **Strategy on the test:**
> 1. Read carefully — note **constraints** (N ≤ 10⁵? 10⁹? 10¹⁸?)
> 2. Constraints decide the approach:
>    - N ≤ 10⁶ → O(N log N) ok
>    - N ≤ 10⁹ → need O(√N) or O(log N)
>    - N ≤ 10¹⁸ → only O(log N), bigint, matrix exp
> 3. Watch for **modulo** — output `% (10⁹ + 7)` is a hint to use modular math
> 4. Check **overflow** before submission — `long long`!
> 5. **Dry run** with edge cases: 0, 1, prime, perfect square, max value

**You've got this! 🚀**
