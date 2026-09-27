
# Phase 1: Arrays & Hashing





## Patterns seekhe is phase mein

```
1. CHAMPION      -> ek dabba, "abhi tak ka best" yaad rakhna
2. TWO POINTERS   -> do dabbe (left, right), dono taraf se andar aana
3. HASHMAP/SET     -> "pehle dekho, phir yaad rakho", O(1) check/store
4. LEFT-RIGHT      -> do arrays banao (left product, right product), phir jodo
```

---

# PROBLEM 1 (Warm-up): Find Maximum in Array

**Question:** Array `[3, 9, 2, 7]` diya hai. Isme sabse bada number dhundo. (Answer: 9)

**Pattern:** Champion / Running Max

## Idea (Approach)

```
Ek dabba (variable) banao jisme "abhi tak dekha hua sabse bada number" rakhenge.
Isko CHAMPION naam dete hain.

1. Array ke pehle number ko champion bana do (shuru mein)
2. Har agle number (CHALLENGER) ko champion se compare karo
3. Challenger champion se BADA hai -> champion badal do
4. Challenger champion se CHHOTA hai -> kuch mat karo, champion wahi rahega
5. Poora array dekhne ke baad, jo champion mein bacha, wahi sabse bada number hai
```

**Real-life example:** Ek race mein 4 log daude. Tu ek ek karke unka time dekhta hai aur "abhi tak ka sabse tez" yaad rakhta hai. Jab koi usse bhi tez nikle, to record badal jaata hai.

## Code (RUNNABLE)

```javascript
// Problem: Array mein sabse bada number dhundna
// Pattern: Champion / running max
// Time: O(n)   Space: O(1)

function findMax(arr) {
  let champion = arr[0];   // pehle number se shuru karo

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > champion) {   // abhi wala number champion se bada hai?
      champion = arr[i];        // haan, to champion badal do
    }
  }

  return champion;
}

// RUNNABLE: neeche wali lines VS Code/terminal mein chalake dekh
console.log(findMax([3, 9, 2, 7]));       // Output: 9
console.log(findMax([-5, -2, -9]));        // Output: -2
console.log(findMax([7]));                  // Output: 7
```

## FULL Dry Run: `arr = [3, 9, 2, 7]`

```
Shuru:  champion = arr[0] = 3

i=1: arr[1] = 9
     PEHLE (compare se pehle): champion = 3
     Compare: 9 > 3? haan
     BAAD MEIN (compare ke baad): champion = 9

i=2: arr[2] = 2
     PEHLE: champion = 9
     Compare: 2 > 9? nahi
     BAAD MEIN: champion = 9   (wahi raha, badla nahi)

i=3: arr[3] = 7
     PEHLE: champion = 9
     Compare: 7 > 9? nahi
     BAAD MEIN: champion = 9   (wahi raha, badla nahi)

Final Answer = 9   [correct]
```

**GOLDEN RULE:** Is round ka "PEHLE" = pichli round ka "BAAD MEIN". Champion SIRF tab badalta hai jab challenger usse BADA ho.

## Complexity

```
Time  = O(n)    -> har number ko ek baar dekha
Space = O(1)    -> sirf 2 variables (champion, i)
```

## Kya isse FAST ho sakta hai?

**Nahi.** Agar koi number bina dekhe chhod diya, to wahi sabse bada ho sakta tha. Har element dekhna ZAROORI hai. Interview line: *"Har element dekhna zaroori hai, isliye O(n) hi optimal hai."*

## Edge Cases

```
[7]              -> 7 (ek hi element hai)
[-5, -2, -9]     -> -2 (negatives bhi chalte hain, isliye champion=arr[0] rakha, 0 nahi)
[]  (khaali)     -> arr[0] undefined hoga -> interview mein poochna: "khaali ho sakta hai?"
```

---

# PROBLEM 2 (Warm-up): Second Largest

**Question:** Array `[3, 9, 2, 7]` diya hai. Isme DOOSRA sabse bada number dhundo. (Answer: 7)

**Pattern:** Do dabbe (first, second)

## Brute Force (pehla simple idea)

```
Step 1: findMax() chalao poora array pe    -> sabse bada mila (9)
Step 2: us number (9) ko array se hata do   -> naya array [3, 2, 7]
Step 3: bache hue array pe findMax() dobara chalao -> 7 mila
Complexity: findMax 2 baar chala -> kaam ~ 2n -> constant hatao -> O(n)
```

## Better Idea (ek hi loop mein)

```
Do dabbe rakho:
  first  = abhi tak ka sabse bada
  second = abhi tak ka doosra sabse bada

Har challenger pe 3 CASES:
CASE 1: challenger > first
        -> purana "first" ab "second" ban jayega
        -> naya challenger "first" banega
        (order zaroori: PEHLE second=first, PHIR first=challenger)
CASE 2: challenger > second (par first se chhota)
        -> sirf "second" badlega
CASE 3: challenger dono se chhota
        -> kuch nahi hoga
```

**IMPORTANT — order kyu matter karta hai:** Ek dabba ek time pe sirf EK number rakh sakta hai. Agar PEHLE `first = challenger` kar de, to purana `first` **mit jayega**, aur `second = first` karne pe NAYA number dono jagah aa jayega (galat!).

```
GALAT order:  first = challenger;  second = first;   (dono mein same aa jaata hai)
SAHI order:   second = first;      first = challenger;
```

## Code (RUNNABLE)

```javascript
// Problem: Array mein doosra sabse bada number
// Pattern: Do dabbe (first, second)
// Time: O(n)   Space: O(1)

function secondLargest(arr) {
  let first = arr[0];
  let second = -Infinity;   // "khaali" ke liye -- sabse chhota possible number

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > first) {
      second = first;         // purana first, second ban gaya
      first = arr[i];          // naya first
    } else if (arr[i] > second) {
      second = arr[i];         // sirf second badla
    }
  }

  return second;
}

// RUNNABLE:
console.log(secondLargest([5, 2, 8, 6]));   // Output: 6
console.log(secondLargest([3, 9, 2, 7]));    // Output: 7
```

## `-Infinity` kya hai (detail)

Ye ek special JavaScript value hai, matlab "sabse chhota number jo ho sakta hai". Koi bhi real number isse bada hota hai. Shuru mein "second" ka koi value nahi pata, isliye `-Infinity` rakha, taaki pehla hi challenger ise aasani se replace kar de.

## FULL Dry Run: `arr = [5, 2, 8, 6]`

```
Shuru:  first = 5,  second = -Infinity

i=1: arr[1] = 2
     2 > first(5)?  nahi
     2 > second(-Infinity)?  haan
     -> second = 2
     |  first=5, second=2

i=2: arr[2] = 8
     8 > first(5)?  haan
     -> second = first = 5   (purana first, second bana)
     -> first = arr[i] = 8    (naya first)
     |  first=8, second=5

i=3: arr[3] = 6
     6 > first(8)?  nahi
     6 > second(5)?  haan
     -> second = 6
     |  first=8, second=6

Final Answer = second = 6   [correct]
```

**Check:** `[5,2,8,6]` mein bade se chhote: 8, 6, 5, 2. Sabse bada 8, doosra 6. Match!

**GOLDEN CHECK RULE:** `first` HAMESHA `second` se bada ya barabar hona chahiye.

## Complexity

```
Time  = O(n)   -> ek hi loop
Space = O(1)   -> sirf first, second, i
```

---

# PROBLEM 3 (Warm-up): Reverse Array

**Question:** Array `[1, 2, 3, 4, 5]` ko ulta karo: `[5, 4, 3, 2, 1]`. In-place (extra array nahi).

**Pattern:** Two Pointers (NAYA pattern, Champion se ALAG)

## Two Pointers kya hai

```
Champion:      EK pointer, EK disha mein (0 se end tak)
Two Pointers:  DO pointers, DONO taraf se EK SAATH andar aate hain

left  -> shuru se chalta hai, AAGE badhta hai (left+1)
right -> aakhir se chalta hai, PEECHE aata hai (right-1)
```

## Position vs Value

```
Array:      [5, 2, 3, 4, 1]
Position:    0  1  2  3  4     <- INDEX, 0 se shuru

arr[i] = us POSITION pe jo VALUE rakhi hai, seedha padhna hai
```

## `arr.length - 1` kyu

```
arr.length      = kitne elements hain (COUNT)
arr.length - 1  = LAST element ka INDEX
```
5 elements ho to index 0-4 tak jaate hain, isliye last index = length - 1 = 4.

## temp variable kya hai

Asthaayi dabba, thodi der ke liye number yaad rakhne wala. Do glass (paani, juice) swap karne ke liye teesra (temp) glass chahiye, warna ek content kho jaata hai.

```
GALAT (temp ke bina): arr[left] = arr[right]  (purana arr[left] kho gaya)
SAHI (temp ke saath): temp=arr[left]; arr[left]=arr[right]; arr[right]=temp;
```

## Code (RUNNABLE)

```javascript
// Problem: Array ko reverse karo (in-place)
// Pattern: Two Pointers
// Time: O(n)   Space: O(1)

function reverseArray(arr) {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    let temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left = left + 1;
    right = right - 1;
  }

  return arr;
}

// RUNNABLE:
console.log(reverseArray([1, 2, 3, 4, 5]));   // Output: [ 5, 4, 3, 2, 1 ]
console.log(reverseArray([1, 2]));             // Output: [ 2, 1 ]
console.log(reverseArray([7]));                 // Output: [ 7 ]
```

## FULL Dry Run: `arr = [1, 2, 3, 4, 5]`

```
Shuru:  left=0, right=4,  arr=[1,2,3,4,5]

--- Round 1 ---
temp = arr[left] = arr[0] = 1
arr[left] = arr[right]  -> arr[0]=arr[4]=5   -> arr=[5,2,3,4,5]
arr[right] = temp        -> arr[4]=1          -> arr=[5,2,3,4,1]
left = 0+1 = 1
right = 4-1 = 3

--- Round 2 ---
Check: left<right? 1<3 -> haan, chalega
temp = arr[left] = arr[1] = 2
arr[left] = arr[right]  -> arr[1]=arr[3]=4   -> arr=[5,4,3,4,1]
arr[right] = temp        -> arr[3]=2          -> arr=[5,4,3,2,1]
left = 1+1 = 2
right = 3-1 = 2

--- Check ---
left<right? 2<2 -> NAHI, loop RUK gaya

Final Answer = [5, 4, 3, 2, 1]   [correct]
```

**Note:** Beech ka element (`3`, position 2) kabhi CHHUA nahi gaya, kyunki length ODD thi. Rule: `left < right`, `left <= right` NAHI.

## Complexity

```
Time  = O(n)   -> har element lagbhag ek baar touch (n/2 swaps, constant hata do)
Space = O(1)   -> koi naya array nahi banaya ("in-place")
```

---

# PROBLEM 4: Two Sum

**Question:** Array `[2, 7, 11, 15]`, target `9`. Do numbers dhundo jinka sum target ho. (Answer: `2, 7`)

**Pattern:** HashMap ("pehle dekho, phir yaad rakho")

## Brute Force

```
Har number ko har DOOSRE number se jodke dekho.
n=4 pe -> 4x4=16 pairs (worst case) -> O(n^2)
(Question B jaisa hi pattern)
```
`O(n^2)` slow hai bade arrays ke liye. Fast tareeka chahiye.

## HashMap kya hai (naya data structure)

Phonebook example: naam se seedha jump karke number nikaalna (O(1)), poori list padhne (O(n)) ke bajaye.

```javascript
let map = new Map();
map.set("Ramesh", 9876543210);   // set: O(1)
map.get("Ramesh");                 // get: O(1)
map.has("Ramesh");                 // has: O(1)
```

## Idea

```
Formula: need = target - arr[i]

Har number pe:
 1. need nikalo
 2. map.has(need)? haan -> jodi mil gayi, return [need, arr[i]]
 3. nahi -> map.set(arr[i], true), aage badho
```

## Code (RUNNABLE)

```javascript
// Problem: Array mein do numbers dhundo jinka sum target ho
// Pattern: HashMap
// Time: O(n)   Space: O(n)

function twoSum(arr, target) {
  let map = new Map();

  for (let i = 0; i < arr.length; i++) {
    let need = target - arr[i];

    if (map.has(need)) {
      return [need, arr[i]];
    }

    map.set(arr[i], true);
  }
}

// RUNNABLE:
console.log(twoSum([2, 7, 11, 15], 9));    // Output: [ 2, 7 ]
console.log(twoSum([3, 2, 4], 6));           // Output: [ 2, 4 ]
```

## FULL Dry Run: `arr = [2, 7, 11, 15]`, `target = 9`

```
map = {} (khaali)

i=0: arr[0] = 2
     need = target - arr[i] = 9 - 2 = 7
     map.has(7)? map khaali hai -> NAHI
     -> map.set(2, true)   |  map = {2}

i=1: arr[1] = 7
     need = 9 - 7 = 2
     map.has(2)? map mein 2 hai -> HAAN!
     -> return [need, arr[i]] = [2, 7]

Final Answer = [2, 7]   [correct]
```

## Complexity

```
Time  = O(n)    -> har number ek baar, map.has()/map.set() dono O(1)
Space = O(n)    -> map mein extra numbers store hote hain

Time vs Space trade-off: extra memory use karke time bachaya (O(n^2) -> O(n))
```

---

# PROBLEM 5: Contains Duplicate

**Question:** Array `[1, 2, 3, 1]` mein koi number repeat hua hai kya? (Answer: true)

**Pattern:** Set

## Brute Force

Har number ko har doosre se compare karo -> O(n^2), same as Two Sum brute force.

## Set kya hai

Map jaisa, bas sirf "hai ya nahi" check karna hai, key-value nahi.

```javascript
let seen = new Set();
seen.add(5);        // O(1)
seen.has(5);         // true, O(1)
```

## Idea

```
Har number pe:
 1. seen.has(arr[i])? haan -> duplicate mil gaya -> return true
 2. nahi -> seen.add(arr[i]), aage badho
Poora array chal gaya, kuch nahi mila -> return false
```

## Code (RUNNABLE)

```javascript
// Problem: Array mein koi duplicate hai ya nahi
// Pattern: Set
// Time: O(n)   Space: O(n)

function containsDuplicate(arr) {
  let seen = new Set();

  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) {
      return true;
    }
    seen.add(arr[i]);
  }

  return false;
}

// RUNNABLE:
console.log(containsDuplicate([1, 2, 3, 1]));   // Output: true
console.log(containsDuplicate([1, 2, 3, 4]));    // Output: false
```

## FULL Dry Run: `arr = [1, 2, 3, 1]`

```
seen = {} (khaali)

i=0: arr[0]=1,  seen.has(1)?  seen khaali hai -> nahi
     -> seen.add(1)  ->  seen = {1}

i=1: arr[1]=2,  seen.has(2)?  set {1} mein 2 nahi -> nahi
     -> seen.add(2)  ->  seen = {1, 2}

i=2: arr[2]=3,  seen.has(3)?  set {1,2} mein 3 nahi -> nahi
     -> seen.add(3)  ->  seen = {1, 2, 3}

i=3: arr[3]=1,  seen.has(1)?  set {1,2,3} mein 1 HAI -> HAAN!
     -> return true

Final Answer = true   [correct]
```

**Doosra example (duplicate NAHI):** `[1,2,3,4]` -> saare "nahi" milte hain -> loop poora chalta hai -> `return false`

## Complexity

```
Time  = O(n)    -> har number ek baar, seen.has()/seen.add() dono O(1)
Space = O(n)    -> worst case poore array ke numbers set mein
```

---

# PROBLEM 6: Product of Array Except Self

**Question:** Array `[1, 2, 3, 4]`. Har position ke liye baaki saare numbers ka product nikaalo (us position ka number chhod ke). Division use NAHI karna (interview rule). (Answer: `[24, 12, 8, 6]`)

**Pattern:** Left product x Right product

## Brute Force

```
Har position ke liye ek naya loop chalao jo baaki sab multiply kare -> O(n^2)
(Bahar loop n baar, andar bhi n baar = Question B jaisa pattern)
```

## Optimized Idea

```
Har position ka answer = (LEFT ke saare numbers ka product) x (RIGHT ke saare numbers ka product)
LEFT ya RIGHT khaali ho to uska product = 1 (multiplication mein "kuch nahi" = 1)
```

**Poora naksha:**
```
Position 0 (val=1): LEFT=khaali        RIGHT=2,3,4
Position 1 (val=2): LEFT=1              RIGHT=3,4
Position 2 (val=3): LEFT=1,2            RIGHT=4
Position 3 (val=4): LEFT=1,2,3          RIGHT=khaali
```

## leftProducts array kaise banta hai (running product)

```
leftProducts[0] = 1                                  (khaali LEFT)
leftProducts[i] = leftProducts[i-1] * arr[i-1]        (pichla LEFT x naya number)
```

**Dry run leftProducts, `arr=[1,2,3,4]`:**
```
leftProducts[0] = 1
leftProducts[1] = leftProducts[0] * arr[0] = 1 * 1 = 1
leftProducts[2] = leftProducts[1] * arr[1] = 1 * 2 = 2
leftProducts[3] = leftProducts[2] * arr[2] = 2 * 3 = 6

leftProducts = [1, 1, 2, 6]
```

## rightProducts array kaise banta hai (ulti taraf se)

```
rightProducts[n-1] = 1                                  (khaali RIGHT, last position)
rightProducts[i] = rightProducts[i+1] * arr[i+1]         (agle wale ka RIGHT x agle wale ka number)
```

**Dry run rightProducts, `arr=[1,2,3,4]`:**
```
rightProducts[3] = 1
rightProducts[2] = rightProducts[3] * arr[3] = 1 * 4 = 4
rightProducts[1] = rightProducts[2] * arr[2] = 4 * 3 = 12
rightProducts[0] = rightProducts[1] * arr[1] = 12 * 2 = 24

rightProducts = [24, 12, 4, 1]
```

**NOTE (common confusion):** `rightProducts[3]=1` mein jo "1" hai, wo PLACEHOLDER hai (humne khud banaya "khaali ka product"). Ye array ka number `arr[0]=1` se ALAG hai — koi connection nahi.

## Dono jodna

```
answer[i] = leftProducts[i] * rightProducts[i]

answer[0] = 1 * 24 = 24
answer[1] = 1 * 12 = 12
answer[2] = 2 * 4  = 8
answer[3] = 6 * 1  = 6

Final Answer = [24, 12, 8, 6]
```

## Code (RUNNABLE)

```javascript
// Problem: har position ke liye baaki sab numbers ka product (bina division)
// Pattern: Left product x Right product
// Time: O(n)   Space: O(n)

function productExceptSelf(arr) {
  let n = arr.length;
  let leftProducts = new Array(n);
  let rightProducts = new Array(n);
  let answer = new Array(n);

  // Left products: left se right jaate hue
  leftProducts[0] = 1;
  for (let i = 1; i < n; i++) {
    leftProducts[i] = leftProducts[i - 1] * arr[i - 1];
  }

  // Right products: right se left jaate hue
  rightProducts[n - 1] = 1;
  for (let i = n - 2; i >= 0; i--) {
    rightProducts[i] = rightProducts[i + 1] * arr[i + 1];
  }

  // Dono jodo
  for (let i = 0; i < n; i++) {
    answer[i] = leftProducts[i] * rightProducts[i];
  }

  return answer;
}

// RUNNABLE:
console.log(productExceptSelf([1, 2, 3, 4]));   // Output: [ 24, 12, 8, 6 ]
console.log(productExceptSelf([2, 3, 4, 5]));    // Output: [ 60, 40, 30, 24 ]
```

## Complexity

```
Time  = O(n)   -> 3 alag loops, ek ke baad ek (n+n+n = 3n -> O(n))
Space = O(n)    -> 3 extra arrays banaye (leftProducts, rightProducts, answer)
```

---

## Mistake Log — Phase 1

```
1. Code mein FIX number likh diya (champion = 9)
   -> NAAM likhna chahiye (arr[i]), warna code sirf EK specific array pe chalega

2. "champion = champion" likh diya (kuch badla hi nahi)
   -> jeetne wale ko dabbe mein rakhna hai: champion = arr[i]

3. Dry run mein dabbe ko KHUD SE compare kar diya (jaise 9 > 9)
   -> hamesha DO ALAG numbers compare karne hain

4. left/right ko VALUE samjha, POSITION nahi samjha
   -> left aur right hamesha INDEX hote hain

5. temp ke bina seedha swap karne ki koshish ki
   -> purani value overwrite hone se kho jaati hai, temp zaroori hai

6. Two Sum formula mein order ulta kiya
   -> hamesha target - arr[i], ulta karne se galat answer aata hai

7. rightProducts[3]=1 wale "1" ko array ke number "1" (arr[0]) se confuse kiya
   -> ye do alag cheezein hain: ek "placeholder" hai, doosra array ka real number

8. Function BANANA aur function CALL karna alag cheez hai
   -> sirf "function findMax(arr) {...}" likhne se kuch print nahi hota
   -> console.log(findMax([3,9,2,7])) likhna padta hai use CHALANE ke liye
```

---

