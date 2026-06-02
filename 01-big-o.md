# Big-O Notation & Time Complexity

## Big-O คืออะไร?

Big-O คือวิธีวัดว่า **algorithm ช้าลงแค่ไหน เมื่อ input ใหญ่ขึ้น**

ไม่ได้วัดเวลาจริง (วินาที) แต่วัด "อัตราการเติบโต" ของงานที่ต้องทำ

> ถ้า input เพิ่มขึ้น 10 เท่า → งานเพิ่มขึ้นกี่เท่า?

---

## ลำดับความเร็ว (เร็ว → ช้า)

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)
```

---

## แต่ละ Complexity หมายความว่าอะไร

### O(1) — Constant
ไม่ว่า input จะใหญ่แค่ไหน งานเท่าเดิมเสมอ

```js
const first = arr[0]  // เข้าถึง index ตรงๆ ไม่ต้องวนลูป
```

### O(log n) — Logarithmic
ทุกรอบ ตัด input ออกครึ่งนึง → งานเพิ่มช้ามาก

```
input 8  → ทำ 3 รอบ  (8 → 4 → 2 → 1)
input 16 → ทำ 4 รอบ
input 1M → ทำแค่ ~20 รอบ
```

### O(n) — Linear
ยิ่ง input มาก ยิ่งทำงานมากตามสัดส่วน

```js
for (let i = 0; i < arr.length; i++) {
  // วนครบทุก element
}
```

### O(n log n) — Linearithmic
เกิดจากการ "แบ่งครึ่ง" แต่ต้องวน n รอบในแต่ละชั้น
พบมากใน sorting ที่ดี เช่น Merge Sort, Quick Sort

### O(n²) — Quadratic
มี loop ซ้อน loop → input เพิ่ม 2 เท่า งานเพิ่ม 4 เท่า

```js
for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length; j++) {
    // ทำงาน n × n ครั้ง
  }
}
```

### O(2ⁿ) — Exponential
เพิ่ม input แค่ 1 → งานเพิ่มเป็น 2 เท่า อันตราย

```js
// Fibonacci แบบ naive recursive
function fib(n) {
  return fib(n - 1) + fib(n - 2)  // แตกเป็น 2 branches ทุกครั้ง
}
```

---

## กฎการวิเคราะห์

### 1. ตัด constant ออก
```
O(2n)    → O(n)
O(500)   → O(1)
O(13n²)  → O(n²)
```

### 2. เก็บแค่ term ที่ใหญ่สุด
```
O(n² + n)       → O(n²)
O(n + log n)    → O(n)
```

### 3. Loop ซ้อน = คูณ
```
O(n) loop ใน O(n) loop = O(n²)
```

### 4. Code ต่อกัน = บวก แล้วค่อย simplify
```
O(n) + O(n) = O(2n) → O(n)
O(n) + O(n²) = O(n² + n) → O(n²)
```

---

## Space Complexity

วัดเหมือนกัน แต่วัด **memory** ที่ใช้ ไม่ใช่เวลา

```js
// O(1) space — ไม่สร้าง array ใหม่
function sum(arr) {
  let total = 0
  for (let n of arr) total += n
  return total
}

// O(n) space — สร้าง array ใหม่ขนาด n
function double(arr) {
  return arr.map(n => n * 2)
}
```

---

## สรุปเร็ว

| Complexity | ชื่อ | รู้สึกยังไงเมื่อ n = 1,000,000 |
|------------|------|-------------------------------|
| O(1) | Constant | ทำทันที |
| O(log n) | Logarithmic | ~20 steps เท่านั้น |
| O(n) | Linear | 1 ล้าน steps |
| O(n log n) | Linearithmic | ~20 ล้าน steps |
| O(n²) | Quadratic | 1 ล้านล้าน steps (ช้ามาก) |
| O(2ⁿ) | Exponential | ทำไม่ได้เลย |
