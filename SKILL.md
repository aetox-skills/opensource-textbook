---
name: opensource-textbook
description: >
  ใช้เมื่อให้ AI อ่าน วิเคราะห์ และแปลง Open Source repository
  เป็นเอกสารเชิงตำราเรียนภาษาไทย โดยต้องตรวจสอบ repo จริง
  แยกข้อเท็จจริงจากการอนุมาน อธิบายสถาปัตยกรรมจาก codebase
  และจัดลำดับเนื้อหาให้ผู้อ่านเรียนรู้จากพื้นฐานไปสู่การลงมือทำ
---

# 📖 OpenSource Textbook — จาก Repo สู่ตำราเรียน

> สกิลนี้ไม่ใช่แค่แม่แบบเอกสาร แต่เป็น **ชั้นวินัย** สำหรับแปลง Open Source
> ให้เป็นหลักสูตรที่สอนคน — จากไม่รู้ → เข้าใจ → วิเคราะห์ได้ → ลงมือทำได้

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
10. **ห้ามเขียน architecture chapter จาก README อย่างเดียว** — ต้องผ่าน Code Reading Pipeline ทั้ง 7 ขั้น
11. **หยุดเมื่อพอดี** — ไม่ใช่ทุกโปรเจคต้องตำรา 15 sections (ดู Pass Levels)
12. **ห้ามสร้างเอกสารตกแต่ง** — ทุก section ต้องมีประโยชน์ต่อการเรียนรู้

---

## Learning Path Rules (4 ข้อ — ตำราทุกบทต้องตอบ)

ก่อนเขียนแต่ละบท ให้ตอบ 4 คำถามนี้:

| # | คำถาม | ตัวอย่าง |
|---|-------|---------|
| 1 | **ผู้เรียนต้องรู้อะไรมาก่อน?** | "ต้องรู้ Python เบื้องต้น, HTTP, JSON" |
| 2 | **บทนี้สอนแนวคิดอะไร?** | "สอนว่า Session Management ทำงานอย่างไรใน SQLite" |
| 3 | **หลังอ่านจบ ผู้เรียนควรทำอะไรได้?** | "ออกแบบ session manager ของตัวเองได้" |
| 4 | **ถ้าจะลงมือทดลอง ต้องเริ่มจากไฟล์ไหน?** | "`packages/core/src/session/index.ts` → `createSession()`" |

---

## Chapter Types (6 แบบ)

เลือกประเภทบทให้เหมาะกับเนื้อหา:

### 🧠 Concept Chapter
สอนแนวคิดหลัก เช่น plugin system, auth flow, scheduler

```
ประกอบด้วย:
1. เป้าหมายบทเรียน
2. แนวคิดคืออะไร — อธิบายด้วยภาษาง่าย
3. ทำไมต้องใช้แนวคิดนี้ — ปัญหาที่มันแก้
4. แนวคิดนี้ทำงานอย่างไร — diagram + flow
5. มีใช้ที่ไหนใน repo — ชี้ไฟล์จริง
6. ข้อดี/ข้อเสียของแนวคิดนี้
7. จุดที่มือใหม่มักเข้าใจผิด
8. คำถามชวนคิด
```

### 🏗️ Architecture Chapter
สอนภาพรวมระบบและการแยก component

```
ประกอบด้วย:
1. เป้าหมายบทเรียน
2. ภาพรวมระบบ — diagram ใหญ่
3. แต่ละ component ทำอะไร — ทีละตัว
4. Component คุยกันอย่างไร — data flow
5. ทำไมถึงออกแบบแบบนี้ — tradeoffs
6. ไฟล์สำคัญของแต่ละ component
7. ถ้าจะเปลี่ยน architecture ต้องแก้ตรงไหน
8. คำถามชวนคิด
```

### 🔬 Code Reading Chapter
พาผู้อ่านเดินอ่านไฟล์จริงทีละส่วน

```
ประกอบด้วย:
1. เป้าหมายบทเรียน — "เรากำลังจะอ่านไฟล์ X เพื่อเข้าใจ Y"
2. ภาพรวมของไฟล์ — มันอยู่ในระบบตรงไหน
3. เดินอ่านทีละฟังก์ชัน/class — อธิบายบรรทัดต่อบรรทัด
4. จุดที่ซับซ้อน — อธิบายเพิ่ม
5. สรุปสิ่งที่เรียนรู้จากไฟล์นี้
6. แบบฝึกหัด — "ลองแก้ฟังก์ชัน X ให้ทำ Y แทน"
```

### 🔨 Implementation Chapter
พาผู้อ่านสร้าง mini version จากแนวคิดของ repo

```
ประกอบด้วย:
1. เป้าหมาย — "เราจะสร้าง X แบบง่าย"
2. แนวคิดจาก repo ตัวจริง — อธิบาย core idea
3. Implementation แบบ mini — code พร้อมอธิบาย
4. ต่างจากของจริงตรงไหน — อะไรที่ตัดออก, ทำไม
5. วิธีรัน/ทดสอบ
6. แบบฝึกหัด — "เพิ่ม feature Y เข้าไป"
```

### ⚖️ Comparison Chapter
เทียบ design choice กับทางเลือกอื่น

```
ประกอบด้วย:
1. เป้าหมาย — "เราจะเทียบ X กับ Y"
2. เกณฑ์การเปรียบเทียบ — อะไรคือปัจจัยสำคัญ
3. ตารางเปรียบเทียบ
4. วิเคราะห์ — แต่ละตัวเหมาะกับอะไร
5. ทำไม repo นี้เลือกแบบนี้
6. คำถามชวนคิด — "ถ้าเปลี่ยนเป็น Y จะเกิดอะไรขึ้น?"
```

### ❓ Exercise Chapter
ทดสอบความเข้าใจท้ายบท

```
ประกอบด้วย:
1. สรุปแนวคิดสำคัญของบทก่อนหน้า
2. แบบฝึกหัด 3-5 ข้อ
   - ข้อ 1: ทบทวนความเข้าใจ
   - ข้อ 2: วิเคราะห์
   - ข้อ 3: ลงมือทำ
3. เฉลย (แยก section)
4. สิ่งที่ควรอ่านต่อ
```

---

## Code Reading Pipeline (7 ขั้น — ห้ามข้าม)

ก่อนเขียนบทเรียนจาก repo **ต้องอ่านตามลำดับนี้เท่านั้น**:

```
1. README.md          → เข้าใจเป้าหมายโปรเจกต์
         │
2. package/config      → เข้าใจ stack, dependencies, build system
         │
3. entrypoint          → เข้าใจจุดเริ่มระบบ, main/cli/server
         │
4. core modules        → เข้าใจแกนหลัก — ฟังก์ชัน/class หลัก
         │
5. adapters/integrations → เข้าใจการเชื่อมต่อภายนอก — API, DB, MCP
         │
6. tests/examples      → เข้าใจ intended usage — วิธีที่ควรใช้
         │
7. docs/issues/releases → เข้าใจบริบท, ข้อจำกัด, history
```

### Pipeline Rules

- **ห้ามข้ามขั้น** — ต้องอ่านครบก่อนเริ่มเขียน architecture chapter
- **ห้ามเขียน architecture chapter จาก README อย่างเดียว** — README คือขั้นที่ 1 จาก 7
- ทุกขั้นต้องบันทึกสิ่งที่พบ: `[✓]` เห็นจากไฟล์, `[→]` อนุมาน, `[!]` ยังไม่ได้อ่าน
- ถ้าขั้นไหนอ่านไม่ถึง (เช่น tests/ ไม่มี) → ทำเครื่องหมาย `[!] ไม่พบ tests`

---

## Teaching Output Format (9 ส่วน)

ทุกบทต้องมี 9 ส่วนนี้ — **นี่คือหัวใจของตำราเรียน**:

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

### 🔍 Quick Note (สำหรับสำรวจ)
- 1-2 sections, compact
- ใช้เมื่อ: repo เล็ก, low-risk, preview
- ไม่ต้องครบ 9 ส่วน

### 📄 Standard Lesson (สำหรับสอน)
- 5-8 sections, 9 ส่วนครบ
- ใช้เมื่อ: concept เดียว, module เดียว
- มีแบบฝึกหัด

### 📘 Full Textbook Chapter (สำหรับหลักสูตร)
- 10-15 sections, 9 ส่วนครบ + Deep Dive + Comparison
- ใช้เมื่อ: repo ใหญ่, foundation concept, หลาย module
- มี Exercise Chapter แยก

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
- [ ] ตอบ Learning Path 4 ข้อแล้ว
- [ ] หรือ Early Exit: `ไม่ต้องสอน — แค่ compact note`

### Gate 2: Inspection — Code Reading Pipeline
- [ ] README.md อ่านแล้ว `[✓]`
- [ ] package/config อ่านแล้ว `[✓]`
- [ ] entrypoint อ่านแล้ว `[✓]`
- [ ] core modules อ่านแล้ว `[✓]`
- [ ] adapters อ่านแล้ว `[✓]`
- [ ] tests/examples ดูแล้ว `[✓]` หรือ `[!] ไม่พบ`
- [ ] docs/issues ดูแล้ว `[✓]` หรือ `[!]`

### Gate 3: Classification
- [ ] Chapter Type ตรงกับเนื้อหา
- [ ] ระบุหมวดหมู่ของระบบแล้ว

### Gate 4: Question
- [ ] ระบุสิ่งที่ยังไม่รู้และสำคัญ
- [ ] หรือ `ไม่มีคำถามที่ส่งผล`

### Gate 5: Mapping
- [ ] Diagram เชื่อมโยงกับ evidence ได้
- [ ] ทุก component มี `[✓]` `[→]` `[?]` `[!]`

### Gate 6: Validation
1. Claim Traceability — ทุกข้อความมีแหล่งอ้างอิง?
2. Scope Alignment — ตรงกับ intake?
3. Handoff Readiness — มี unknowns + next actions?

---

## Anti-Patterns

| ❌ อย่าทำ | ✅ ควรทำ |
|-----------|---------|
| เขียน architecture chapter จาก README อย่างเดียว | ผ่าน Code Reading Pipeline 7 ขั้น |
| ไม่มีหัวข้อ "จุดที่มือใหม่มักเข้าใจผิด" | มีทุกบท |
| ไม่มีแบบฝึกหัด | มี 3-5 ข้อทุกบท |
| Throw ข้อมูลโดยไม่จัดลำดับ | Learning Path → Concept → Architecture → Practice |
| ภาษาอังกฤษปนไทยโดยไม่จำเป็น | ไทยให้มากที่สุด |
| ใช้ stars เก่า | `[✓]` stars จริงจาก GitHub |
| เดาสถาปัตยกรรม | `[→]` อนุมาน หรือ `[!]` ยังไม่ยืนยัน |
| Architecture chapter ทุกโปรเจค | เลือก Chapter Type ให้เหมาะ |

---

## Checklist ก่อนส่งงาน

```
□ ทุกบทตอบ Learning Path 4 ข้อ
□ ผ่าน Code Reading Pipeline 7 ขั้น
□ มี 9 ส่วน Teaching Format ครบ (สำหรับ Standard+)
□ เลือก Chapter Type ถูกต้อง
□ Evidence Strength กำกับทุกข้อความสำคัญ
□ Architecture diagram โยงถึงไฟล์ได้
□ มี "จุดที่มือใหม่มักเข้าใจผิด"
□ มีแบบฝึกหัด 3-5 ข้อ
□ ภาษาไทยทั้งหมด (ยกเว้นชื่อ, code, URL)
□ [✓] stars/language จริงจาก GitHub
```

---

## Templates

เลือก template จาก `templates/` ตาม Chapter Type:

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
