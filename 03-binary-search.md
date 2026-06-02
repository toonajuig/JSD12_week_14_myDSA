# Binary Search

## ไอเดียหลัก

Binary Search คือการค้นหาใน **array ที่ sort แล้ว** โดยการ **ตัดครึ่ง** ทุกรอบ

แทนที่จะดูทีละตัวจากซ้าย → ดูตรงกลางก่อน แล้วตัดสินใจว่าจะหาซ้ายหรือขวา

> เหมือนหาคำในพจนานุกรม — เปิดกลาง ถ้าคำที่หาอยู่หลัง ก็ตัดครึ่งซ้ายออก

---

## Time & Space Complexity

| | Complexity |
|---|---|
| Time (Best) | O(1) — เจอที่ตำแหน่งกลางทันที |
| Time (Average/Worst) | O(log n) |
| Space (Iterative) | O(1) |
| Space (Recursive) | O(log n) — call stack |

**เปรียบเทียบ**: array 1,000,000 ตัว
- Linear Search → ดูสูงสุด 1,000,000 ครั้ง
- Binary Search → ดูสูงสุด **20 ครั้ง** (log₂ 1,000,000 ≈ 20)

---

## วิธีทำงาน

```
หา 7 ใน [1, 3, 5, 7, 9, 11, 13]
             ↑         ↑         ↑
           left       mid      right

mid = arr[3] = 7 ✓ เจอแล้ว!

---

หา 5 ใน [1, 3, 5, 7, 9, 11, 13]
left=0, right=6, mid=3 → arr[3]=7 > 5 → หาซ้าย (right=2)
left=0, right=2, mid=1 → arr[1]=3 < 5 → หาขวา (left=2)
left=2, right=2, mid=2 → arr[2]=5 = 5 ✓ เจอ!
```

---

## Implementation

### Iterative (แนะนำ — Space O(1))

```js
function binarySearch(arr, target) {
  let left = 0
  let right = arr.length - 1

  while (left <= right) {
    const mid = Math.floor((left + right) / 2)

    if (arr[mid] === target) return mid       // เจอ → return index
    if (arr[mid] < target) left = mid + 1    // target อยู่ขวา
    else right = mid - 1                      // target อยู่ซ้าย
  }

  return -1  // ไม่เจอ
}
```

### Recursive (เข้าใจ concept ดีกว่า แต่ใช้ Stack)

```js
function binarySearch(arr, target, left = 0, right = arr.length - 1) {
  if (left > right) return -1  // base case: ไม่เจอ

  const mid = Math.floor((left + right) / 2)

  if (arr[mid] === target) return mid
  if (arr[mid] < target) return binarySearch(arr, target, mid + 1, right)
  return binarySearch(arr, target, left, mid - 1)
}
```

---

## ข้อควรระวัง

**ต้อง sort ก่อนเสมอ** — ถ้า array ไม่ได้ sort ผลลัพธ์ผิดแน่นอน

```js
// ผิด — array ยังไม่ sort
binarySearch([5, 3, 8, 1], 3)  // อาจ return -1 ทั้งที่ 3 อยู่ใน array

// ถูก
binarySearch([1, 3, 5, 8], 3)  // return 1 ✓
```

**Mid calculation** — อย่าใช้ `(left + right) / 2` ใน low-level language เพราะ integer overflow
ใน JS ไม่มีปัญหา แต่รู้ไว้ดีกว่า: safe version คือ `left + Math.floor((right - left) / 2)`

---

## เมื่อไหร่ควรใช้ Binary Search?

```
✓ array/list ถูก sort แล้ว
✓ ต้องการหาค่าหรือตำแหน่งใน O(log n)
✓ ข้อมูลใหญ่มาก linear search ช้าเกินไป

✗ array ไม่ได้ sort (ต้องเสีย O(n log n) sort ก่อน → อาจไม่คุ้ม)
✗ ข้อมูลเล็กมาก (linear search ง่ายกว่า เร็วพอ)
```

---

## Binary Search บน "เงื่อนไข" (แบบ advanced)

บางครั้งใช้ Binary Search ไม่ใช่แค่หาค่า แต่หา **จุดเปลี่ยน** ของเงื่อนไข

```
หา index แรกสุดที่ arr[i] >= target
หา จำนวน minimum ที่ทำให้เงื่อนไขเป็น true
```

Pattern นี้เรียกว่า "Binary Search on Answer" — พบบ่อยในโจทย์ competitive programming
