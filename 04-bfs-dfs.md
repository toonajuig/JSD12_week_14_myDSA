# BFS & DFS (Graph Traversal)

## Graph คืออะไร?

Graph คือโครงสร้างที่มี **node (vertex)** และ **edge** เชื่อมกัน

```
    A
   / \
  B   C
 / \
D   E
```

แทนด้วย Adjacency List ใน JS:
```js
const graph = {
  A: ['B', 'C'],
  B: ['D', 'E'],
  C: [],
  D: [],
  E: []
}
```

---

## Complexity ของทั้งสอง

| | Time | Space |
|---|---|---|
| BFS | O(V + E) | O(V) |
| DFS | O(V + E) | O(V) |

- **V** = จำนวน vertices (nodes)
- **E** = จำนวน edges
- O(V + E) เพราะต้องเยี่ยมทุก node และทุก edge อย่างน้อยครั้งหนึ่ง

---

## BFS — Breadth-First Search

### ไอเดีย
เดินทีละชั้น (level) ก่อน — เยี่ยมทุก neighbor ของ node ปัจจุบันก่อน แล้วค่อยไปชั้นถัดไป

ใช้ **Queue** (FIFO) เป็นตัวช่วย

### ลำดับการเดิน
```
    A          เริ่มที่ A
   / \
  B   C        ชั้น 1: B, C
 / \
D   E          ชั้น 2: D, E

ลำดับ: A → B → C → D → E
```

### Implementation
```js
function bfs(graph, start) {
  const visited = new Set()
  const queue = [start]
  const result = []

  visited.add(start)

  while (queue.length > 0) {
    const node = queue.shift()  // ดึงออกจากหน้า queue
    result.push(node)

    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor)
        queue.push(neighbor)   // เพิ่มเข้าท้าย queue
      }
    }
  }

  return result
}
```

---

## DFS — Depth-First Search

### ไอเดีย
เดินลึกสุดก่อน — ไปตาม path จนสุดทาง แล้วค่อย backtrack กลับมาหาทาง branching ถัดไป

ใช้ **Stack** (LIFO) หรือ **Recursion**

### ลำดับการเดิน
```
    A          เริ่มที่ A
   / \
  B   C        ไป B ก่อน
 / \
D   E          ไปลึกสุด D ก่อน แล้ว backtrack ไป E แล้วค่อยไป C

ลำดับ: A → B → D → E → C
```

### Implementation (Recursive)
```js
function dfs(graph, node, visited = new Set()) {
  visited.add(node)
  const result = [node]

  for (const neighbor of graph[node]) {
    if (!visited.has(neighbor)) {
      result.push(...dfs(graph, neighbor, visited))
    }
  }

  return result
}
```

### Implementation (Iterative — ใช้ Stack)
```js
function dfs(graph, start) {
  const visited = new Set()
  const stack = [start]
  const result = []

  while (stack.length > 0) {
    const node = stack.pop()  // ดึงออกจากหลัง stack

    if (visited.has(node)) continue
    visited.add(node)
    result.push(node)

    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        stack.push(neighbor)
      }
    }
  }

  return result
}
```

---

## BFS vs DFS เลือกแบบไหน?

| สถานการณ์ | ใช้อะไร | ทำไม |
|-----------|---------|------|
| หา shortest path (unweighted) | **BFS** | BFS รับประกันว่าเจอ path ที่สั้นที่สุดก่อน |
| ตรวจว่า node เชื่อมกันไหม | ใช้ได้ทั้งคู่ | ไม่ต่างกัน |
| หา cycle ใน graph | **DFS** | ง่ายกว่ากับ DFS recursive |
| Topological Sort | **DFS** | ใช้ post-order traversal |
| Maze solving | **DFS** | ลองทางจนสุดแล้ว backtrack |
| Social network (คนที่ใกล้ที่สุดก่อน) | **BFS** | traverse ทีละ degree |

---

## ข้อควรระวัง

**Visited set สำคัญมาก** — ถ้าไม่ mark visited จะวนซ้ำไม่หยุด (infinite loop)

```
A ↔ B  (เชื่อมสองทาง)
ถ้าไม่ mark visited: A→B→A→B→A→... วนตลอด
```

**DFS recursive กับ graph ใหญ่** — อาจ stack overflow เพราะ call stack ลึกมาก
ถ้า graph ใหญ่ ควรใช้ DFS iterative (ใช้ stack ของเราเอง)

---

## สรุปภาพรวม

```
BFS  =  Queue  =  เดินทีละชั้น  =  หา shortest path
DFS  =  Stack/Recursion  =  ดำดิ่งลงไปก่อน  =  explore/backtrack
```
