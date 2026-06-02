# Sorting Algorithms

## ภาพรวม

| Algorithm | Best | Average | Worst | Space | Stable? |
|-----------|------|---------|-------|-------|---------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |

> **Stable** = ถ้า element ค่าเท่ากัน ลำดับเดิมจะยังคงอยู่หลัง sort

---

## Bubble Sort

### ไอเดีย
วนซ้ำ เปรียบเทียบคู่ข้างกัน ถ้าซ้ายมากกว่าขวา → swap
ทำซ้ำจนไม่มีการ swap → เรียงเรียบร้อย

element ที่ใหญ่สุดจะ "ลอยขึ้น" ไปอยู่ท้ายสุดทุก pass

### ตัวอย่าง
```
[5, 3, 8, 1]
pass 1: [3, 5, 1, 8]  → 8 ไปอยู่ท้าย
pass 2: [3, 1, 5, 8]  → 5 ไปอยู่ที่ตำแหน่งถูกต้อง
pass 3: [1, 3, 5, 8]  → เรียบร้อย
```

```js
function bubbleSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]  // swap
      }
    }
  }
  return arr
}
```

**ใช้เมื่อ**: array เล็กมาก หรือเรียนรู้ concept เท่านั้น ไม่เหมาะ production

---

## Selection Sort

### ไอเดีย
หา element ที่เล็กสุดใน array ที่เหลือ แล้ว swap มาไว้ข้างหน้า
ทำซ้ำจนครบ

### ตัวอย่าง
```
[5, 3, 8, 1]
pass 1: หา min=1 → swap กับ index 0 → [1, 3, 8, 5]
pass 2: หา min=3 → อยู่ที่เดิม    → [1, 3, 8, 5]
pass 3: หา min=5 → swap กับ index 2 → [1, 3, 5, 8]
```

```js
function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minIndex = i
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[minIndex]) minIndex = j
    }
    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]]  // swap
    }
  }
  return arr
}
```

**ใช้เมื่อ**: ต้องการ minimize จำนวน swap (swap แค่ n-1 ครั้ง)

---

## Insertion Sort

### ไอเดีย
เหมือนจัดไพ่ในมือ — หยิบ element ใหม่ แล้วเสียบเข้าตำแหน่งที่ถูกต้องใน "ส่วนที่ sort แล้ว"

### ตัวอย่าง
```
[5, 3, 8, 1]
[5 | 3, 8, 1] → หยิบ 3 เสียบหน้า 5  → [3, 5 | 8, 1]
[3, 5 | 8, 1] → หยิบ 8 ใส่ท้ายได้เลย → [3, 5, 8 | 1]
[3, 5, 8 | 1] → หยิบ 1 เสียบหน้าสุด  → [1, 3, 5, 8]
```

```js
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    const current = arr[i]
    let j = i - 1
    while (j >= 0 && arr[j] > current) {
      arr[j + 1] = arr[j]  // เลื่อนไปขวา
      j--
    }
    arr[j + 1] = current  // เสียบตำแหน่งที่ถูกต้อง
  }
  return arr
}
```

**ใช้เมื่อ**: array เกือบ sort แล้ว (best case O(n)) หรือ array เล็ก

---

## Merge Sort

### ไอเดีย
**Divide & Conquer** — แบ่งครึ่ง จนเหลือ array ขนาด 1 แล้วค่อย merge กลับ
การ merge สองอาร์เรย์ที่ sort แล้ว → ง่ายมาก แค่เปรียบเทียบหัว

```
[5, 3, 8, 1]
   ↓ แบ่ง
[5, 3]  [8, 1]
   ↓ แบ่งอีก
[5][3]  [8][1]
   ↓ merge
[3, 5]  [1, 8]
   ↓ merge
[1, 3, 5, 8]
```

```js
function mergeSort(arr) {
  if (arr.length <= 1) return arr

  const mid = Math.floor(arr.length / 2)
  const left = mergeSort(arr.slice(0, mid))
  const right = mergeSort(arr.slice(mid))

  return merge(left, right)
}

function merge(left, right) {
  const result = []
  let l = 0, r = 0

  while (l < left.length && r < right.length) {
    if (left[l] <= right[r]) result.push(left[l++])
    else result.push(right[r++])
  }

  return [...result, ...left.slice(l), ...right.slice(r)]
}
```

**ใช้เมื่อ**: ต้องการ stable sort ที่รับประกัน O(n log n) เสมอ, array ใหญ่

---

## Quick Sort

### ไอเดีย
เลือก **pivot** (ตัวอ้างอิง) แล้วจัดให้
- ตัวเล็กกว่า pivot → ไปซ้าย
- ตัวใหญ่กว่า pivot → ไปขวา

แล้วทำซ้ำ (recurse) กับซ้ายและขวา

```
[5, 3, 8, 1]  pivot = 5
  ↓
[3, 1] [5] [8]   → 3,1 น้อยกว่า 5 | 8 มากกว่า 5
  ↓
[1, 3] [5] [8]
  ↓
[1, 3, 5, 8]
```

```js
function quickSort(arr) {
  if (arr.length <= 1) return arr

  const pivot = arr[arr.length - 1]
  const left = arr.slice(0, -1).filter(x => x <= pivot)
  const right = arr.slice(0, -1).filter(x => x > pivot)

  return [...quickSort(left), pivot, ...quickSort(right)]
}
```

**ใช้เมื่อ**: ต้องการ average performance ดีที่สุด, cache-friendly
**ระวัง**: worst case O(n²) เมื่อ array sort แล้วและเลือก pivot แย่

---

## เลือก Sorting Algorithm ยังไง?

```
array เล็กมาก (< 20)?        → Insertion Sort
ต้องการ stable?              → Merge Sort
ต้องการ memory น้อย?         → Quick Sort หรือ Heap Sort
array เกือบ sort แล้ว?       → Insertion Sort
ต้องการ worst case O(n log n)? → Merge Sort
ทั่วไป (built-in sort)?      → ภาษาส่วนใหญ่ใช้ Timsort (Merge + Insertion)
```
