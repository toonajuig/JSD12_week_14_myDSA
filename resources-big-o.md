# แหล่งเรียนรู้ Big-O Notation

แนะนำดูตามลำดับนี้: **3 → 2 → 1** (เข้าใจ why ก่อน → ดู code จริง → ทบทวนเร็ว)

---

## 1. Big-O Notation in 100 Seconds
**Channel:** Fireship
**Link:** https://www.youtube.com/watch?v=g2o22C3CRfU

คลิปสั้น 100 วินาที style Fireship — เน้น visual เร็วมาก เหมาะเปิดซ้ำ

**สิ่งที่สอน:**
- Big-O คือการวัด "อัตราการเติบโต" ของ algorithm ไม่ใช่เวลาจริง
- โฟกัส worst-case scenario เสมอ
- ครอบคลุม O(1), O(log n), O(n), O(n log n), O(n²) พร้อม visual
- กฎ drop constants และ drop non-dominant terms

**เหมาะกับ:** คนที่อยากได้ภาพรวมก่อน หรือทบทวนแบบเร็ว

---

## 2. Learn Big O Notation in 6 Minutes
**Channel:** Bro Code
**Link:** https://www.youtube.com/watch?v=XMUe3zFhM5c

คลิปเข้าใจง่ายที่สุดใน 3 คลิป เน้น code ตัวอย่างจริงทุก complexity

**สิ่งที่สอน:**
- อธิบาย Big-O ด้วยโค้ดจริงแต่ละแบบ
- ครอบคลุมครบ: O(1) → O(log n) → O(n) → O(n log n) → O(n²) → O(2ⁿ) → O(n!)
- เน้น "จำนวน steps ที่ต้องทำ" ไม่ใช่เวลา
- ให้เห็นว่า input โตขึ้น steps โตตามยังไง

**เหมาะกับ:** คนเพิ่งเริ่ม อยากเห็น code จริงประกอบทุก complexity

---

## 3. Basic Big O เรื่องที่ทำให้เด็กมหาลัยหลายๆคนปวดหัว !!!
**Channel:** ภาษาไทย
**Link:** https://www.youtube.com/watch?v=lWHbg1SrUqc

คลิปภาษาไทย เน้นตอบคำถามว่า "ทำไมต้องสนใจ Big-O?"

**สิ่งที่สอน:**
- code ที่ output เหมือนกัน แต่ performance ต่างกันมาก — Big-O คือคำตอบ
- Big-O = worst-case ของ algorithm
- ยกตัวอย่างจริง: optimization ทำให้ server เดิมรองรับ user ได้ 8-10 เท่า โดยไม่ต้องเพิ่ม spec
- อธิบายว่า dev ต้องเลือก algorithm ให้เหมาะกับ trade-off

**เหมาะกับ:** อยากเข้าใจ "ทำไม Big-O ถึงสำคัญในชีวิตจริง" เป็นภาษาไทย
