
# Phase 0: Big O (Time & Space Complexity)

---

## Big O kya hota hai

Big O ek tareeka hai ye batane ka ki **input ka size (n) badhne par code ka kaam kitni tezi se badhta hai.**

Ye **seconds nahi napta** -- kyunki alag alag computer alag speed ke hote hain, ek purana laptop slow hoga, ek naya fast. Isliye Big O **exact time nahi**, balki **growth (badhne ka pattern)** napta hai.

**Factory wali example:** Maan lo ek factory mein 1000 parts hain, aur tujhe un sabko ek ek karke check karna hai ki koi kharab to nahi. Agar tu ek ek part uthake check kare, to 1000 parts ke liye 1000 checks lagenge. Agar koi **smart tareeka** ho jisse kam checks mein kaam ho jaye, to wahi behtar hai. **DSA ka poora matlab hi yahi hai -- smart tareeke dhundhna.**

## DSA kya hai (full form)

```
D + S = Data Structure  -> data ko RAKHNE ka tareeka (jaise Array, HashMap, Stack, Tree)
A     = Algorithm       -> data PE KAAM karne ka step-by-step tareeka (jaise loop chalana, compare karna)
```

Example: `[3, 9, 2, 7]` ek **Data Structure** (Array) hai. Usme loop chala ke sabse bada number dhundhna ek **Algorithm** hai.

## Time Complexity vs Space Complexity

```
Time Complexity  = code kitna KAAM karta hai (n badhne ke saath)
Space Complexity = code kitni EXTRA memory leta hai (input ko iss mein nahi ginte)
```

Agar code sirf 2-3 variables use kare, chahe array 10 ka ho ya 10 lakh ka, to Space O(1) hai. Agar code ek naya array banaye jitna bada original hai, to Space O(n) hai.

## Complexity nikaalne ki TRICK (har question mein yahi use karna hai)

```
Step 1: Ek CHHOTA n lo (jaise 4 ya 8)
Step 2: Code ko HAATH SE chalao (dry run), gino kaam kitna hua
Step 3: Ab n ko DOUBLE karo, phir se gino kaam kitna hua
Step 4: Dekho kaam KITNE GUNA badha
```

Is "kitne guna" se naam pata chalta hai:

```
kaam 2 guna hua     ->  O(n)        [seedha badhta hai]
kaam 4 guna hua     ->  O(n^2)      [square ki tarah badhta hai]
kaam sirf +1 step   ->  O(log n)    [bahut slow badhta hai, super fast algorithm]
kaam bilkul nahi    ->  O(1)        [n se farak hi nahi padta]
```

## Chhote se bade tak (poori list)

```
O(1)  <  O(log n)  <  O(n)  <  O(n log n)  <  O(n^2)  <  O(2^n)  <  O(n!)
```

Jitna left mein, utna FAST. Jitna right mein, utna SLOW (bade n ke liye).

## Interview mein n ki limit dekh ke guess karna (cheat sheet)

| Agar n ki limit ho | To ye complexity chalegi |
|---|---|
| n <= 10 | O(n!) bhi chal jayega |
| n <= 20 | O(2^n) chalega |
| n <= 500 | O(n^3) chalega |
| n <= 5,000 | O(n^2) chalega |
| n <= 1,00,000 | O(n log n) chahiye |
| n <= 10,00,000 | O(n) chahiye |
| n >= 10^9 (arab) | O(log n) ya O(1) chahiye |

Iska matlab: agar interviewer bole "n 1 lakh tak ja sakta hai", to tujhe pata chal jana chahiye ki O(n^2) wala solution **fail** ho jayega (bahut slow), aur O(n log n) ya usse fast kuch chahiye.

## Big O SIMPLIFY karne ke 4 RULES

**Rule 1: Constant hatao**
Agar kaam `2n` hai, to Big O mein "2" hata ke sirf `O(n)` likhte hain.
```
2n -> O(n)
5n -> O(n)
100n -> O(n)
```

**Rule 2: Chhota term hatao**
Agar kaam `n^2 + n` hai, to bade n ke liye `n^2` hissa itna bada ho jata hai ki `n` wala hissa ignore ho jata hai.
```
n^2 + n -> O(n^2)
```
Kyu? n = 1000 pe: n^2 = 10,00,000 aur n = 1000. n^2 wala hissa 1000 GUNA bada hai.

**Rule 3: Loop ke ANDAR loop -> MULTIPLY hota hai**
```
for i:
  for j:
    kaam
```
Ye `n x n = n^2` deta hai.

**Rule 4: Loop ke BAAD loop (ek ke baad ek) -> ADD hota hai**
```
for i:
  kaam
for j:
  kaam
```
Ye `n + n = 2n` deta hai, jo simplify hoke `O(n)` ban jata hai.

---

## Warm-up Example: SUM function

```javascript
// Problem: Array ke saare numbers ka sum nikalo
// Pattern: Simple loop
// Time: O(n)   Space: O(1)

function sum(arr) {
  let total = 0;
  for (let i = 0; i < arr.length; i++) {
    total = total + arr[i];
  }
  return total;
}
```

**Dry run** `arr = [2, 5, 3]`:

```
Round 1: total = 0, arr[0] = 2  ->  total = 0 + 2 = 2
Round 2: total = 2, arr[1] = 5  ->  total = 2 + 5 = 7
Round 3: total = 7, arr[2] = 3  ->  total = 7 + 3 = 10
Answer = 10
```

**GOLDEN RULE:** Har round ka "PEHLE wala" number = pichle round ka "BAAD MEIN wala" number.

```
3 elements    -> loop 3 baar chala
1000 elements -> loop 1000 baar chala
```
n jitna, kaam utna -> **O(n)**

---

## Question A

```
for i in 0..n:
    print(i)
```

`print(i)` loop ke ANDAR hai, isliye har round mein chalti hai, total `n` baar.
```
n = 5    -> 5 baar
n = 1000 -> 1000 baar
```
**A -> O(n)**

---

## Question B

```
for i in 0..n:
    for j in 0..n:
        print(i, j)
```

Andar wala loop har baar 0 se shuru hota hai.

`n = 3`:
```
i=0 -> j=0,1,2 -> 3 baar
i=1 -> j=0,1,2 -> 3 baar
i=2 -> j=0,1,2 -> 3 baar
Total = 9
```

**Shortcut formula:** rows x har row ka kaam = n x n
```
n=10  -> 100
n=100 -> 10,000
```

n ko 10 guna kiya, A ka kaam 10 guna hua, B ka kaam **100 guna** hua.

**B -> O(n^2)**

---

## Question C

```
i = n
while i > 1:
    i = i / 2
```

`n=16`: 16->8->4->2->1 = 4 steps
`n=32`: 5 steps
`n=64`: 6 steps

n DOUBLE hua, steps sirf +1 badhe.

**Mechanical example:** Rod ko baar baar BEECH se kaatna. 16 unit = 4 cuts, 32 unit = 5 cuts.

```
n = 1,000       -> ~10 steps
n = 10,00,000   -> ~20 steps
```

**C -> O(log n)**

---

## Question D

```
for i in 0..n:
    for j in i..n:
        print(i, j)
```

Andar wala loop `0` se nahi, `i` se shuru hota hai.

`n=5`:
```
i=0 -> j=0,1,2,3,4 -> 5 baar
i=1 -> j=1,2,3,4   -> 4 baar
i=2 -> j=2,3,4     -> 3 baar
i=3 -> j=3,4       -> 2 baar
i=4 -> j=4         -> 1 baar
Total = 5+4+3+2+1 = 15
```

**Formula:** `(n/2) x (n+1) = n^2/2 + n/2`

```
n=1000 -> 5,00,500
n=2000 -> 20,01,000   (4 guna)
```

B ka lagbhag AADHA hai, par growth PATTERN same (n^2) hai.

**D -> O(n^2)**

---

## Question E

```
for i in 0..n:
    print(i)
for j in 0..n:
    print(j)
```

Loops ek ke BAAD ek hain (nested nahi) -> ADD karte hain.

```
n=10: 10+10 = 20
n=20: 20+20 = 40
```

n double, kaam 2 guna -> `n+n = 2n` -> constant hatao -> `n`

**Mechanical example:** 1 belt = 10 min, 2 belts (ek ke baad ek) = 20 min.

**E -> O(n)**

---

## SUMMARY

```
A -> O(n)
B -> O(n^2)
C -> O(log n)
D -> O(n^2)
E -> O(n)


A: Simple loop
javascript
function questionA(n) {
  for (let i = 0; i < n; i++) {
    console.log(i);
  }
}

questionA(5);
// Output: 0, 1, 2, 3, 4 (5 baar print hoga)





B: Nested loop (loop ke andar loop)
javascript
function questionB(n) {
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      console.log(i, j);
    }
  }
}

questionB(3);
// Output: 9 lines (0,0) (0,1) (0,2) (1,0) (1,1) (1,2) (2,0) (2,1) (2,2)









C: Aadha hone wala loop
javascript
function questionC(n) {
  let i = n;
  while (i > 1) {
    console.log(i);
    i = i / 2;
  }
}

questionC(16);
// Output: 16, 8, 4, 2 (4 lines print hongi)












D: Nested loop (j, i se shuru)
javascript
function questionD(n) {
  for (let i = 0; i < n; i++) {
    for (let j = i; j < n; j++) {
      console.log(i, j);
    }
  }
}

questionD(5);
// Output: 15 lines total (dekhte jaana ki i badhne pe kam j print hote hain)












E: Do loops ek ke baad ek
javascript
function questionE(n) {
  for (let i = 0; i < n; i++) {
    console.log(i);
  }
  for (let j = 0; j < n; j++) {
    console.log(j);
  }
}

questionE(10);
// Output: pehle 0-9 (i wala loop), phir dobara 0-9 (j wala loop) = 20 lines



