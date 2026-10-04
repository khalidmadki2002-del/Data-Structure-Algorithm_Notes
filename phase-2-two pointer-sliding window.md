
# Phase 2: Two Pointers & Sliding Window


```

## Naye Patterns is phase mein
```
1. TWO POINTERS (naya use)  -> array ke dono kinaron se shuru, CHHOTI/BEHTAR
                                wali side ko move karna (DECISION lena, sirf swap nahi)
2. SLIDING WINDOW            -> ek "window" (continuous hissa) array/string pe
                                khisakti hai, bina baar baar poora recalculate kiye
```

---

# PROBLEM 1: Container With Most Water

**Question:** Heights ka array diya hai. Do lines chuno jo saath mein SABSE ZYADA paani rok sakein.

```
heights = [1, 8, 6, 2, 5, 4, 8, 3, 7]
index:      0  1  2  3  4  5  6  7  8
```

Answer: **49**

## Idea

```
Container ka Area = (chhoti wali line ki height) x (width, dono lines ke beech ki doori)
area = Math.min(height[left], height[right]) * (right - left)
```

## Brute Force
```
Har do lines (i, j) ka jodi test karo -> O(n^2)
```

## Optimized Idea (Two Pointers)
```
left = 0, right = n-1, champion = 0

Jab tak left < right:
1. Area calculate karo
2. champion se bada hai to champion badal do
3. CHHOTI wali line ko andar khisako (left ya right, jo chhoti hai)
4. Dohrao
```

**Kyu chhoti wali line move karte hain:** Area hamesha CHHOTI height se limited hota hai. Badi wali ko move karne se naya area purane se bada nahi ho sakta (chhoti height abhi bhi limit karegi, width bhi kam hoga). Sirf chhoti wali move karne se koi lambi line milne ka mauka hai.

## Code
```javascript
// Time: O(n)   Space: O(1)
function maxArea(height) {
  let left = 0;
  let right = height.length - 1;
  let champion = 0;

  while (left < right) {
    let area = Math.min(height[left], height[right]) * (right - left);
    if (area > champion) {
      champion = area;
    }

    if (height[left] < height[right]) {
      left++;
    } else {
      right--;
    }
  }

  return champion;
}

console.log(maxArea([1, 8, 6, 2, 5, 4, 8, 3, 7]));   // Output: 49
console.log(maxArea([1, 1]));                           // Output: 1
```

## Full Dry Run: `[1, 8, 6, 2, 5, 4, 8, 3, 7]`
```
champion = 0

Round 1: left=0(h=1), right=8(h=7)
         width=8, chhoti=min(1,7)=1, area=1*8=8
         champion=8 (8>0)
         chhoti: left(1<7) -> left++

Round 2: left=1(h=8), right=8(h=7)
         width=7, chhoti=min(8,7)=7, area=7*7=49
         champion=49 (49>8)
         chhoti: right(8>=7) -> right--

Round 3: left=1(h=8), right=7(h=3)
         width=6, chhoti=min(8,3)=3, area=3*6=18
         champion=49 (18<49, badla nahi)
         chhoti: right(8>=3) -> right--

Round 4: left=1(h=8), right=6(h=8)
         width=5, chhoti=min(8,8)=8, area=8*5=40
         champion=49 (40<49)
         chhoti: barabar hai, right-- (convention: left>=right to right move)

... (loop continue hota hai, champion 49 hi rehta hai) ...

Final Answer = 49
```

## Complexity — DETAILED

**Time = O(n)**
Sirf EK loop, `left` aur `right` dono mil ke poora array EK BAAR cover karte hain (brute force ke O(n²) se behtar).

**Space = O(1)**
Sirf `left`, `right`, `champion` — 3 fixed variables.

---

# PROBLEM 2: Best Time to Buy and Sell Stock

**Question:** Stock prices ka array diya hai (har din ka price). Ek din KHAREEDO, baad ke kisi din BECHO, max PROFIT nikalo.

```
prices = [7, 1, 5, 3, 6, 4]
```
Answer: **5** (din 1 pe 1 price pe khareedo, din 4 pe 6 price pe becho: 6-1=5)

## Idea
```
Champion pattern ka variation:
minPrice = abhi tak ka SABSE SASTA price (jab khareedna best hoga)
maxProfit = abhi tak ka SABSE ZYADA profit

Har din pe:
1. Agar aaj ka price minPrice se KAM hai -> minPrice update karo
2. Warna, aaj bechne pe profit = aaj ka price - minPrice
   Agar ye profit maxProfit se zyada hai -> maxProfit update karo
```

**Kyu ye kaam karta hai:** Hum ek hi pass mein, HAR din ko "agar yahi din becha to profit kitna" check karte hain, aur saath mein "abhi tak sabse sasta kharidne ka din" bhi track karte hain.

## Code
```javascript
// Time: O(n)   Space: O(1)
function maxProfit(prices) {
  let minPrice = prices[0];
  let maxProfit = 0;

  for (let i = 1; i < prices.length; i++) {
    if (prices[i] < minPrice) {
      minPrice = prices[i];
    } else if (prices[i] - minPrice > maxProfit) {
      maxProfit = prices[i] - minPrice;
    }
  }

  return maxProfit;
}

console.log(maxProfit([7, 1, 5, 3, 6, 4]));   // Output: 5
console.log(maxProfit([7, 6, 4, 3, 1]));       // Output: 0 (price hamesha girta hai, profit nahi)
```

## Full Dry Run: `[7, 1, 5, 3, 6, 4]`
```
Shuru: minPrice=7, maxProfit=0

i=1: price=1, 1<7? haan -> minPrice=1
i=2: price=5, 5<1? nahi. profit=5-1=4, 4>0? haan -> maxProfit=4
i=3: price=3, 3<1? nahi. profit=3-1=2, 2>4? nahi -> maxProfit=4 (wahi)
i=4: price=6, 6<1? nahi. profit=6-1=5, 5>4? haan -> maxProfit=5
i=5: price=4, 4<1? nahi. profit=4-1=3, 3>5? nahi -> maxProfit=5 (wahi)

Final Answer = 5
```

## Complexity — DETAILED

**Time = O(n)**
EK hi loop, har din O(1) mein check hota hai (compare + shayad update).

**Space = O(1)**
Sirf `minPrice` aur `maxProfit` — 2 fixed variables. Brute force (har pair of days check karna, O(n²)) ke muqable bahut behtar.

---

# PROBLEM 3: 3Sum

**Question:** Array diya hai. Teen numbers dhundo jinka sum = 0 ho. Saare UNIQUE triplets chahiye (duplicate triplets nahi).

```
nums = [-1, 0, 1, 2, -1, -4]
```
Answer: `[[-1, -1, 2], [-1, 0, 1]]`

## Idea
```
Step 1: Array ko SORT karo (chhote se bade) -> [-4, -1, -1, 0, 1, 2]
Step 2: Har number ko "pehla number" maan ke FIX karo (ek loop se)
Step 3: Baaki do numbers (jinka sum = -(pehla number) ho) Two Pointers se dhundo
        (bilkul Container With Most Water jaisa left/right tareeka)
```

**Kyu sort karna zaroori hai:** Sorted array mein Two Pointers kaam karta hai kyunki humein pata hota hai left badhane se sum BADHEGA, right ghatane se sum GHATEGA — isse hum sahi direction mein move kar sakte hain.

## Code
```javascript
// Time: O(n^2)   Space: O(n) ya O(log n) [sorting ke liye]
function threeSum(nums) {
  nums.sort((a, b) => a - b);   // Step 1: sort karo
  let result = [];

  for (let i = 0; i < nums.length - 2; i++) {
    // Duplicate "pehla number" skip karo
    if (i > 0 && nums[i] === nums[i - 1]) {
      continue;
    }

    let left = i + 1;
    let right = nums.length - 1;

    while (left < right) {
      let sum = nums[i] + nums[left] + nums[right];

      if (sum === 0) {
        result.push([nums[i], nums[left], nums[right]]);
        left++;
        right--;
        // Duplicates skip karo
        while (left < right && nums[left] === nums[left - 1]) left++;
        while (left < right && nums[right] === nums[right + 1]) right--;
      } else if (sum < 0) {
        left++;   // sum badhana hai, chhoti value hatao
      } else {
        right--;  // sum ghatana hai, badi value hatao
      }
    }
  }

  return result;
}

console.log(threeSum([-1, 0, 1, 2, -1, -4]));
// Output: [ [ -1, -1, 2 ], [ -1, 0, 1 ] ]
```

## Full Dry Run: `[-1, 0, 1, 2, -1, -4]`
```
Sorted: [-4, -1, -1, 0, 1, 2]
        0    1   2  3  4  5

i=0: nums[0]=-4. left=1, right=5
     sum = -4+(-1)+2 = -3 <0 -> left++  (left=2)
     sum = -4+(-1)+2 = -3 <0 -> left++  (left=3)
     sum = -4+0+2 = -2 <0 -> left++     (left=4)
     sum = -4+1+2 = -1 <0 -> left++     (left=5)
     left<right? 5<5 nahi -> loop khatam, koi triplet nahi mila for -4

i=1: nums[1]=-1. left=2, right=5
     sum = -1+(-1)+2 = 0! -> result=[[-1,-1,2]]
     left++(3), right--(4)
     sum = -1+0+1 = 0! -> result=[[-1,-1,2],[-1,0,1]]
     left++(4), right--(3) -> left<right false, loop khatam

i=2: nums[2]=-1, nums[2]===nums[1]? haan -> SKIP (duplicate)

i=3: nums[3]=0. left=4,right=5
     sum=0+1+2=3>0 -> right--(4) -> left<right false

Final Answer = [[-1,-1,2], [-1,0,1]]
```

## Complexity — DETAILED

**Time = O(n²)**
```
Sorting: O(n log n)
Bahar wala loop (i): n baar
Andar Two Pointers (left,right): har i ke liye max n baar

Total = O(n log n) + O(n * n) = O(n²)  [n² sabse bada term hai, Rule 2]
```

**Space = O(n) ya O(log n)**
Sorting ke liye JavaScript internally O(log n) extra space leta hai (ya O(n), engine pe depend karta hai). `result` array answer store karta hai (usually ye count nahi karte, kyunki output return karna hi hai).

---

# PROBLEM 4: Longest Substring Without Repeating Characters

**Question:** String diya hai. Sabse LAMBI substring dhundo jisme koi character REPEAT na ho.

```
s = "abcabcbb"
```
Answer: **3** (substring "abc", length 3)

## Idea (Sliding Window — NAYA pattern, pehli baar)

```
"Window" = string ka ek CONTINUOUS hissa, jise hum CHHOTA/BADA karte rehte hain.

left aur right dono 0 se shuru (window khaali)
Set banao jo window ke andar ke characters track kare

Har right ke liye:
1. Agar s[right] window mein (Set mein) PEHLE SE hai:
   -> left ko aage badhao, aur duplicate wala character Set se hatao,
      jab tak duplicate nikal na jaye
2. s[right] ko Set mein daalo
3. Window ki length (right-left+1) ko champion se compare karo
4. right ko aage badhao
```

**Sliding Window kyu better hai brute force se:** Brute force mein har substring (O(n²) substrings) check karte, har ek ko verify karne mein O(n) — total O(n³). Sliding Window mein window sirf AAGE badhti hai (kabhi peeche nahi jaati), isliye total kaam O(n) hi rehta hai.

## Code
```javascript
// Time: O(n)   Space: O(min(n, charset size))
function lengthOfLongestSubstring(s) {
  let seen = new Set();
  let left = 0;
  let champion = 0;

  for (let right = 0; right < s.length; right++) {
    while (seen.has(s[right])) {
      seen.delete(s[left]);
      left++;
    }
    seen.add(s[right]);

    let windowLength = right - left + 1;
    if (windowLength > champion) {
      champion = windowLength;
    }
  }

  return champion;
}

console.log(lengthOfLongestSubstring("abcabcbb"));   // Output: 3
console.log(lengthOfLongestSubstring("bbbbb"));        // Output: 1
console.log(lengthOfLongestSubstring("pwwkew"));        // Output: 3
```

## Full Dry Run: `s = "abcabcbb"`
```
seen={}, left=0, champion=0

right=0: s[0]='a', seen.has('a')? nahi. seen={a}. length=0-0+1=1. champion=1
right=1: s[1]='b', seen.has('b')? nahi. seen={a,b}. length=1-0+1=2. champion=2
right=2: s[2]='c', seen.has('c')? nahi. seen={a,b,c}. length=2-0+1=3. champion=3
right=3: s[3]='a', seen.has('a')? HAAN!
         -> seen.delete(s[left=0]='a'), left=1. seen={b,c}
         -> ab seen.has('a')? nahi (loop ruk gaya)
         seen.add('a') -> seen={b,c,a}
         length=3-1+1=3. champion=3 (wahi)
right=4: s[4]='b', seen.has('b')? HAAN!
         -> seen.delete(s[left=1]='b'), left=2. seen={c,a}
         -> seen.has('b')? nahi
         seen.add('b') -> seen={c,a,b}
         length=4-2+1=3. champion=3

... (pattern continues, championrehta hai 3) ...

Final Answer = 3
```

## Complexity — DETAILED

**Time = O(n)**
`right` pointer sirf AAGE badhta hai, n baar. `left` pointer bhi sirf AAGE badhta hai (kabhi peeche nahi jaata), total milake max n baar move karta hai. Dono mil ke O(n) + O(n) = O(2n) = O(n).

**Space = O(min(n, charset size))**
Set mein max utne characters store honge jitne UNIQUE characters ho sakte hain — ya to string ki length (n) jitne, ya alphabet size (jaise 26 lowercase letters) jitne, jo bhi CHHOTA ho.

---

# PROBLEM 5: Sliding Window Maximum

**Question:** Array aur window size `k` diya hai. Har window (k size ki) ka MAXIMUM nikalo, jaise window khisakti jaye.

```
nums = [1,3,-1,-3,5,3,6,7], k=3
```
Answer: `[3,3,5,5,6,7]`

## Idea (Deque — naya data structure)

```
Deque = "Double Ended Queue" — ek list jisme AAGE aur PEECHE dono taraf se
        add/remove kar sakte ho (JavaScript mein array se hi simulate karte hain)

Deque mein hum INDEXES store karte hain (values nahi), aur DECREASING order
mein rakhte hain (sabse bada hamesha front mein)

Har number pe:
1. Deque ke PEECHE se, jo bhi numbers abhi wale number se CHHOTE hain, unhe
   nikaal do (wo kabhi max nahi ban sakte, bekaar hain)
2. Abhi wale index ko deque ke peeche daal do
3. Agar deque ka FRONT wala index window ke bahar nikal gaya hai, use hata do
4. Jab window poori ban jaye (i >= k-1), deque ka FRONT hi is window ka max hai
```

## Code
```javascript
// Time: O(n)   Space: O(k)
function maxSlidingWindow(nums, k) {
  let deque = [];   // indexes store karenge
  let result = [];

  for (let i = 0; i < nums.length; i++) {
    // Peeche se chhote numbers hatao
    while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
      deque.pop();
    }
    deque.push(i);

    // Front agar window ke bahar hai to hatao
    if (deque[0] <= i - k) {
      deque.shift();
    }

    // Window poori ban gayi to max record karo
    if (i >= k - 1) {
      result.push(nums[deque[0]]);
    }
  }

  return result;
}

console.log(maxSlidingWindow([1,3,-1,-3,5,3,6,7], 3));   // Output: [3,3,5,5,6,7]
```

## Full Dry Run: `nums=[1,3,-1,-3,5,3,6,7]`, k=3
```
deque=[], result=[]

i=0: nums[0]=1. deque khaali, push(0). deque=[0]
     window poori? i>=k-1(2)? 0>=2 nahi

i=1: nums[1]=3. nums[deque.last=0]=1 < 3? haan -> pop(0). deque=[]
     push(1). deque=[1]
     0>=2? nahi

i=2: nums[2]=-1. nums[deque.last=1]=3 < -1? nahi -> push(2). deque=[1,2]
     front(1)<=i-k(2-3=-1)? 1<=-1 nahi
     i>=2? haan -> result=[nums[deque[0]=1]=3] -> result=[3]

i=3: nums[3]=-3. nums[deque.last=2]=-1 < -3? nahi -> push(3). deque=[1,2,3]
     front(1)<=i-k(3-3=0)? 1<=0 nahi
     result.push(nums[1]=3) -> result=[3,3]

i=4: nums[4]=5. pop while smaller: nums[3]=-3<5 pop, nums[2]=-1<5 pop, nums[1]=3<5 pop
     deque=[] -> push(4). deque=[4]
     front(4)<=i-k(4-3=1)? nahi
     result.push(nums[4]=5) -> result=[3,3,5]

... (aise hi aage badhta hai) ...

Final Answer = [3,3,5,5,6,7]
```

## Complexity — DETAILED

**Time = O(n)**
Har index zyada se zyada EK BAAR push hota hai aur EK BAAR pop hota hai deque mein (chahe front se ya back se). Isliye total operations `n` ke around rehte hain, bhale hi andar `while` loop ho — ye "amortized O(n)" kehlata hai (average kiya jaye to O(n)).

**Space = O(k)**
Deque mein max `k` indexes store ho sakte hain (window size jitne).

---



---

## Mistake Log — Phase 2

```
1. Container With Most Water mein BADI wali line move karne ki galti
   -> hamesha CHHOTI wali line move karo, area usi se limited hota hai

2. 3Sum mein duplicate triplets aa jaana
   -> sorted array mein same value wale consecutive elements skip karo

3. Sliding Window mein left ko bhi PEECHE le jaane ki koshish
   -> left hamesha sirf AAGE badhta hai, kabhi peeche nahi (isi se O(n) milta hai)

4. Deque mein VALUES store karne ki galti
   -> hamesha INDEXES store karo, taaki window se bahar nikalna check kar sako
```

---

