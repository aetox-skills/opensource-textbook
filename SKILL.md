---
name: opensource-textbook
description: >
  ใช้เมื่อให้ AI อ่าน วิเคราะห์ และแปลง Open Source repository
  เป็นเอกสารเชิงตำราเรียนภาษาไทย โดยต้องตรวจสอบ repo จริง
  แยกข้อเท็จจริงจากการอนุมาน อธิบายสถาปัตยกรรมจาก codebase
  และจัดลำดับเนื้อหาให้ผู้อ่านเรียนรู้จากพื้นฐานไปสู่การลงมือทำ
---

# Repo-to-Textbook Skill

> สกิลสำหรับให้ AI อ่าน Open Source repository แล้วแปลงเป็นตำราเรียนภาษาไทย
> โดยแยก fact / inference / assumption / unknown และจัดลำดับเนื้อหาให้ผู้อ่านเรียนรู้ได้จริง

อ้างอิงจาก: `BookMind/docs/technical_guide.md` + `senior-architect-agent` + ฟีดแบ็คจากอาจารย์

---

## Operating Rules (กฎ 12 ข้อ — ห้ามละเมิด)

1. **ห้ามเขียนก่อนตรวจสอบ** — ต้อง `webfetch` repo, README, AGENTS.md, package files ก่อนเขียน
2. **ห้ามอ้าง stars/milestones โดยไม่ยืนยัน** — ใช้ตัวเลขจริงจาก GitHub เท่านั้น
3. **ห้ามเดาโดยไม่ทำเครื่องหมาย** — ทุกข้อความที่อนุมานต้องกำกับ `[→]`
4. **แยกข้อเท็จจริง 4 ระดับ** — `[✓]` Direct, `[→]` Inferred, `[?]` Assumed, `[!]` Unverified
5. **ถามก่อนสรุป** — ถ้าขาดข้อมูลสำคัญ ให้ถามผู้ใช้หรือเขียน `[!] ยังไม่ยืนยัน`
6. **ใช้ภาษาไทย** — ยกเว้นชื่อโปรเจค, framework, URL, code
7. **สอน อย่าบอก** — ทุกบทต้องสอนให้คนเข้าใจและทำตามได้, ไม่ใช่แค่ throw ข้อมูล
8. **จัดลำดับสมองคนอ่าน** — จากง่ายไปยาก, จากภาพรวมไปรายละเอียด, จากแนวคิดไป implementation
9. **Diagram โยงถึง source** — ทุก component ต้องอ้างอิงไฟล์หรือ docs ได้
10. **ห้ามเขียน Architecture Chapter จาก README อย่างเดียว** — ต้องผ่าน Code Reading Pipeline ทั้ง 7 ขั้น
11. **หยุดเมื่อพอดี** — ไม่ใช่ทุกโปรเจคต้องตำรา 15 sections (ดู Pass Levels)
12. **ห้ามสร้างเอกสารตกแต่ง** — ทุก section ต้องมีประโยชน์ต่อการเรียนรู้

---

## Learning Path Rules (5 ข้อ — ทุกบทต้องตอบ)

ก่อนเขียนแต่ละบท ให้ตอบ 5 คำถามนี้:

| # | คำถาม | ตัวอย่าง |
|---|-------|---------|
| 1 | **ผู้เรียนต้องรู้อะไรมาก่อน?** | "ต้องรู้ Python เบื้องต้น, HTTP, JSON" |
| 2 | **บทนี้สอนแนวคิดอะไร?** | "สอนว่า Session Management ทำงานอย่างไรใน SQLite" |
| 3 | **หลังอ่านจบ ผู้เรียนควรทำอะไรได้?** | "ออกแบบ session manager ของตัวเองได้" |
| 4 | **แนวคิดนี้เห็นได้จากไฟล์ใดใน repo?** | "`packages/core/src/session/index.ts` → `createSession()`" |
| 5 | **ถ้าจะทดลองเอง ควรเริ่มจากจุดไหน?** | "Clone repo → `cd packages/core` → อ่าน `src/session/`" |

---

## Code Reading Pipeline (7 ขั้น — ห้ามข้าม)

ก่อนเขียนบทเรียนจาก Open Source repository **ต้องอ่านตามลำดับนี้เท่านั้น**:

```
1. README.md              → เข้าใจเป้าหมายและการใช้งานหลัก
         │
2. package/config files   → เข้าใจ stack, dependency, runtime
         │
3. entrypoint             → เข้าใจจุดเริ่มต้นของระบบ
         │
4. core modules           → เข้าใจแกน business / engine / runtime
         │
5. adapters/integrations  → เข้าใจการเชื่อมต่อภายนอก
         │
6. examples/tests         → เข้าใจ intended usage
         │
7. docs/issues/releases   → เข้าใจข้อจำกัดและวิวัฒนาการของโปรเจกต์
```

### ⚠️ กฎเหล็ก

**ห้ามเขียน Architecture Chapter จาก README อย่างเดียว**

README คือขั้นที่ 1 จาก 7 — ต้องอ่านให้ครบก่อนเขียน architecture ทุกครั้ง

---

## Chapter Types (6 แบบ)

### 🧠 Concept Chapter — สอนแนวคิดหลัก
```
1. เป้าหมายบทเรียน
2. แนวคิดคืออะไร — อธิบายด้วยภาษาง่าย
3. ทำไมต้องใช้แนวคิดนี้ — ปัญหาที่มันแก้
4. แนวคิดนี้ทำงานอย่างไร — diagram + flow
5. มีใช้ที่ไหนใน repo — ชี้ไฟล์จริง
6. ข้อดี/ข้อเสีย
7. จุดที่มือใหม่มักเข้าใจผิด
8. คำถามชวนคิด
```

### 🏗️ Architecture Chapter — สอนภาพรวม
```
1. เป้าหมายบทเรียน
2. ภาพรวมระบบ — diagram ใหญ่
3. แต่ละ component ทำอะไร — ทีละตัว
4. Component คุยกันอย่างไร — data flow
5. ทำไมถึงออกแบบแบบนี้ — tradeoffs
6. ไฟล์สำคัญของแต่ละ component
7. ถ้าจะเปลี่ยน architecture ต้องแก้ตรงไหน
8. จุดที่มือใหม่มักเข้าใจผิด
9. แบบฝึกหัด
10. คำถามชวนคิด
```

### 🔬 Code Reading Chapter — พาเดินอ่านทีละไฟล์
```
1. เป้าหมาย — "เรากำลังจะอ่านไฟล์ X เพื่อเข้าใจ Y"
2. ภาพรวมของไฟล์ — มันอยู่ในระบบตรงไหน
3. โครงสร้างไฟล์ — imports, main, helpers
4. เดินอ่านทีละฟังก์ชัน/class
5. จุดที่ซับซ้อน — Deep Dive
6. สรุปสิ่งที่เรียนรู้
7. จุดที่มือใหม่มักเข้าใจผิด
8. แบบฝึกหัด
```

### 🔨 Implementation Chapter — สร้าง mini version
```
1. เป้าหมาย — "เราจะสร้าง X แบบง่าย"
2. แนวคิดจาก repo ตัวจริง
3. Implementation แบบ mini — code
4. ต่างจากของจริงตรงไหน
5. วิธีรัน/ทดสอบ
6. แบบฝึกหัด
```

### ⚖️ Comparison Chapter — เทียบ design choice
```
1. เป้าหมาย — "เทียบ A กับ B"
2. เกณฑ์การเปรียบเทียบ
3. ตารางเปรียบเทียบ
4. วิเคราะห์ — แต่ละตัวเหมาะกับอะไร
5. ทำไม repo นี้เลือกแบบ A
6. คำถามชวนคิด
```

### ❓ Exercise Chapter — แบบฝึกหัดท้ายบท
```
1. สรุปแนวคิดสำคัญ
2. แบบฝึกหัด 3-5 ข้อ (ทบทวน → วิเคราะห์ → ลงมือทำ)
3. เฉลย
4. สิ่งที่ควรอ่านต่อ
```

---

## Teaching Output Format (9 ส่วน)

| # | ส่วน | คำอธิบาย |
|---|------|---------|
| 1 | **เป้าหมายบทเรียน** | ผู้เรียนจะได้อะไรหลังจากอ่านบทนี้ |
| 2 | **ภาพรวมแนวคิด** | สิ่งที่บทนี้จะสอน — 3-5 บรรทัด |
| 3 | **ทำไมโปรเจกต์นี้ออกแบบแบบนี้** | Design rationale — tradeoffs, constraints |
| 4 | **ไฟล์จริงที่เกี่ยวข้อง** | รายการไฟล์ + แต่ละไฟล์ทำอะไร |
| 5 | **อธิบาย flow จากบนลงล่าง** | Step-by-step walkthrough |
| 6 | **จุดที่มือใหม่มักเข้าใจผิด** | Common pitfalls + วิธีหลีกเลี่ยง |
| 7 | **แบบฝึกหัดท้ายบท** | 3-5 ข้อ — ทบทวน, วิเคราะห์, ลงมือทำ |
| 8 | **คำถามชวนคิด** | เปิดให้คิดต่อ — "ถ้าเปลี่ยน X จะเกิดอะไร?" |
| 9 | **สิ่งที่ควรอ่านต่อ** | บทต่อไป, docs เพิ่มเติม, paper |

---

## Pass Levels

| Level | เมื่อไหร่ | ความลึก |
|-------|---------|---------|
| **Quick Note** | repo เล็ก, low-risk, preview | 1-2 sections, ไม่ต้องครบ 9 ส่วน |
| **Standard Lesson** | concept เดียว, module เดียว | 5-8 sections, 9 ส่วนครบ, มีแบบฝึกหัด |
| **Full Textbook** | repo ใหญ่, foundation concept | 10-15 sections + Deep Dive + Comparison + Exercise แยก |

---

## Evidence Strength

| ระดับ | เครื่องหมาย | ใช้เมื่อ |
|-------|-----------|---------|
| **Direct** | `[✓]` | ยืนยันจาก repo, README, docs โดยตรง |
| **Inferred** | `[→]` | อนุมานจากหลักฐาน ยังไม่ยืนยันตรงๆ |
| **Assumed** | `[?]` | ตั้งสมมติฐานเพื่อให้เขียนต่อ |
| **Unverified** | `[!]` | ยังไม่มีหลักฐาน — ต้องกลับมาดู |

---

## Checkpoint Gates

### Gate 1: Intake
- [ ] เลือก Chapter Type แล้ว
- [ ] เลือก Pass Level แล้ว
- [ ] ตอบ Learning Path 5 ข้อแล้ว
- [ ] หรือ Early Exit: `ไม่ต้องสอน — แค่ compact note`

### Gate 2: Inspection — Code Reading Pipeline
- [ ] 1. README.md `[✓]`
- [ ] 2. package/config `[✓]`
- [ ] 3. entrypoint `[✓]`
- [ ] 4. core modules `[✓]`
- [ ] 5. adapters `[✓]`
- [ ] 6. examples/tests `[✓]` หรือ `[!]`
- [ ] 7. docs/issues `[✓]` หรือ `[!]`

### Gate 3: Classification
- [ ] Chapter Type ตรงกับเนื้อหา
- [ ] ระบุหมวดหมู่ของระบบแล้ว

### Gate 4: Question
- [ ] ระบุสิ่งที่ยังไม่รู้ หรือ `ไม่มีคำถามที่ส่งผล`

### Gate 5: Mapping
- [ ] Diagram ทุก component มี `[✓]` `[→]` `[?]` `[!]`

### Gate 6: Validation
1. Claim Traceability — ทุกข้อความมีแหล่งอ้างอิง?
2. Scope Alignment — ตรงกับ intake?
3. Handoff Readiness — มี unknowns + next actions?

---

## Anti-Patterns

| ❌ อย่าทำ | ✅ ควรทำ |
|-----------|---------|
| เขียน Architecture Chapter จาก README อย่างเดียว | ผ่าน Code Reading Pipeline 7 ขั้น |
| ไม่มีหัวข้อ "จุดที่มือใหม่มักเข้าใจผิด" | มีทุกบท |
| ไม่มีแบบฝึกหัด | มี 3-5 ข้อทุกบท |
| Throw ข้อมูลโดยไม่จัดลำดับ | Learning Path → Concept → Architecture → Practice |
| ภาษาอังกฤษปนไทยโดยไม่จำเป็น | ไทยให้มากที่สุด |
| ใช้ stars เก่า | `[✓]` stars จริงจาก GitHub |
| เดาสถาปัตยกรรม | `[→]` อนุมาน หรือ `[!]` ยังไม่ยืนยัน |
| Architecture chapter ทุกโปรเจค | เลือก Chapter Type ให้เหมาะ |
| README.md = SKILL.md (ซ้ำกัน) | README = คน, SKILL.md = AI |

---

## Templates

- `templates/concept-chapter.md`
- `templates/architecture-chapter.md`
- `templates/code-reading-chapter.md`
- `templates/implementation-chapter.md`
- `templates/comparison-chapter.md`
- `templates/exercise-chapter.md`

## Checklists

- `checklists/repo-inspection.md` — 7 ขั้น Code Reading Pipeline
- `checklists/evidence-check.md` — Evidence Strength audit
- `checklists/textbook-quality.md` — 9 ส่วน Teaching Format
