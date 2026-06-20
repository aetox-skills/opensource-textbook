---
name: opensource-textbook
description: >
  ใช้เมื่อให้ AI อ่าน วิเคราะห์ และแปลง Open Source repository
  เป็นเอกสารเชิงตำราเรียนภาษาไทย โดยต้องตรวจสอบ repo จริง
  แยกข้อเท็จจริงจากการอนุมาน อธิบายสถาปัตยกรรมจาก codebase
  และจัดลำดับเนื้อหาให้ผู้อ่านเรียนรู้จากพื้นฐานไปสู่การลงมือทำ
---

# Repo-to-Textbook Skill (v5)

> "อย่าให้ AI เขียนตำราจากความทรงจำ
> ให้มันเขียนจากหลักฐาน แล้วแปลงหลักฐานเป็นบทเรียน"

---

## Operating Rules (กฎ 14 ข้อ)

1. **ห้ามเขียนก่อนตรวจสอบ** — ต้องผ่าน Code Reading Pipeline 7 ขั้นก่อนเขียน
2. **ห้ามอ้าง stars โดยไม่ยืนยัน** — ใช้ตัวเลขจริงจาก GitHub `[✓]`
3. **ห้ามเดาโดยไม่ทำเครื่องหมาย** — `[→]` Inferred, `[?]` Assumed, `[!]` Unverified
4. **แยกข้อเท็จจริง 4 ระดับ** — Direct, Inferred, Assumed, Unverified
5. **ถามก่อนสรุป** — ขาดข้อมูลสำคัญ → ถามผู้ใช้หรือเขียน `[!]`
6. **ใช้ภาษาไทย** — ยกเว้นชื่อโปรเจค, framework, URL, code
7. **สอน อย่าบอก** — ทุกบทต้องสอนให้คนเข้าใจและทำตามได้
8. **แปลง repo structure → learning structure** — คนเรียนไม่ได้คิดตามโครง repo
9. **Diagram โยงถึง source** — ทุก component ต้องอ้างอิงไฟล์หรือ docs ได้
10. **ห้ามเขียน Architecture Chapter จาก README อย่างเดียว** — 7 ขั้น pipeline
11. **หยุดเมื่อพอดี** — ไม่ใช่ทุกโปรเจคต้องตำรา (ดู Pass Levels)
12. **ตรวจตัวเองก่อนส่ง** — คะแนน < 85 → revise (ดู Evaluation Rubric)
13. **ห้ามสร้างเอกสารตกแต่ง** — สร้างเฉพาะไฟล์ที่จำเป็นต่อการเรียนรู้ ไม่ใช่สร้างเพราะ "มี template"
14. **แยกเนื้อหาใหญ่เป็นส่วนย่อย** — 1 บท ≠ 1 repo, 1 บท = 1 แนวคิดที่สอนจบในตัวเอง

---

## 1. Book-Level Planning (ต้องทำก่อนเริ่มเขียนบท)

ก่อนเขียนบทแรกของโปรเจคใดๆ ต้องสร้างไฟล์ `book-plan.md`:

```
book-plan.md
├── target_reader         # Reader Persona (ดู §6)
├── prerequisite_map      # ต้องรู้อะไรก่อนอ่านเล่มนี้
├── chapter_order         # ลำดับบท — เรียงตาม learning structure
├── concept_dependency    # §3 — อะไรต้องรู้ก่อนอะไร
├── repeated_terms        # คำศัพท์ที่ปรากฏซ้ำ → glossary candidates
├── glossary_candidates   # §4 — คำที่จะอธิบายใน glossary
├── project_walkthrough   # §2.3 — เส้นทางพาผู้เรียนลงมือทำ
├── final_learning_outcome # หลังอ่านจบทั้งเล่ม ทำอะไรได้
└── output_files          # files ที่จะสร้าง (บท, glossary, patterns, cheatsheet)
```

### Book-Level Planning Rules
- **ต้องทำก่อนเขียนบทแรก** — ไม่ใช่เขียนเสร็จแล้วค่อยมาวางแผน
- **chapter_order ≠ repo structure** — เรียงตามการเรียนรู้ ไม่ใช่ตามโฟลเดอร์
- **target_reader ต้องเลือกจาก Reader Persona (§6)** — ห้ามใช้ "ทั่วไป"
- **final_learning_outcome ต้องวัดได้** — ไม่ใช่ "เข้าใจระบบ" แต่ "สร้าง X ได้"

### Early Exit Rule

ก่อนเริ่มทำ book-plan ทั้งเล่ม ให้ถามก่อนว่า repo นี้ **ควร** ทำเป็นตำราไหม:

```
ควรทำตำรา เมื่อ:
✅ repo มี architecture ที่น่าสนใจ — design patterns, novel approach
✅ repo มีขนาดเหมาะสม — มากกว่า 500 stars หรือมี paper
✅ repo มี documentation เพียงพอ — อ่าน 7 ขั้น pipeline ได้ครบ

ไม่ควรทำตำรา เมื่อ:
❌ repo เล็กมาก — < 100 stars, < 50 commits
❌ repo คือ boilerplate/example — ไม่มี architecture ให้เรียน
❌ repo deprecated — มี successor แล้ว
❌ อ่าน 7 ขั้น pipeline ไม่ครบ — ข้อมูลไม่พอ
```

**ถ้าไม่ควรทำ:** สร้างเฉพาะ compact note (Quick Note pass) — 1-2 sections — แล้วระบุสาเหตุว่า "ไม่ต้องตำราเพราะ..." ห้ามสร้าง book-plan หรือหลายบท

---

## 2. Knowledge Density Rule (5 ชั้น — ทุกหัวข้อต้องมี)

ทุกหัวข้อสำคัญในแต่ละบทต้องมีความรู้ 5 ชั้น:

| ชั้น | ชื่อ | คำถามที่ตอบ | ถ้าขาด = อะไร |
|------|------|-----------|-------------|
| **1** | Concept | แนวคิดคืออะไร? | อ่านไม่รู้เรื่อง |
| **2** | Mechanism | มันทำงานอย่างไร? | รู้แต่ชื่อ ไม่รู้หลักการ |
| **3** | Source Evidence | repo ใช้ตรงไหน? `[✓]` | เหมือนอ่าน textbook ทั่วไป |
| **4** | Design Reason | ทำไมเขาเลือกแบบนี้? | จำได้ แต่ประยุกต์ไม่เป็น |
| **5** | Transfer Skill | เอาไปใช้กับโปรเจกต์ตัวเองยังไง? | เรียนแล้วใช้ไม่เป็น |

### Density Check (ต่อบท)

```
สำหรับแต่ละหัวข้อ H ในบท:
  ✅ มี 5 ชั้น → ตำรา
  ⚠️ มีแค่ 1-2 + 3 → เอกสารอ่านโค้ด
  ❌ มีแค่ 1-2 → README summary

ก่อนส่งบท: random เลือก 3 หัวข้อ → ตรวจว่ามีครบ 5 ชั้น
ถ้าหัวข้อไหนไม่ครบ → เพิ่มชั้นที่ขาด หรือทำเครื่องหมาย [→] ว่ายังไม่มีหลักฐาน
```

---

## 3. Concept Dependency Graph (โครงตำรา ≠ โครง repo)

**กฎเหล็ก:** AI ต้องแปลงจาก repo structure → learning structure

```
Repo Structure (โครงระบบ):          Learning Structure (โครงตำรา):
├── packages/                        ├── บท 1: แนวคิดพื้นฐาน
│   ├── core/                        ├── บท 2: ระบบทำงานอย่างไร
│   ├── api/                         │   ├── 2.1 ส่วนที่ต้องรู้ก่อน
│   └── plugins/                     │   ├── 2.2 ส่วนที่ต่อจาก 2.1
│                                    │   └── 2.3 ส่วนที่ใช้ 2.1+2.2
                                     ├── บท 3: ลงมือสร้าง
                                     └── บท 4: วิเคราะห์เปรียบเทียบ
```

### วิธีการสร้าง Dependency Graph

```
1. ลิสต์ทุกแนวคิดสำคัญจาก repo
2. สำหรับแต่ละแนวคิด A → ถาม: "ต้องรู้ B ก่อนถึงจะเข้าใจ A ไหม?"
3. ถ้าใช่ → B ต้องมาก่อน A ใน chapter_order
4. วาดเป็นกราฟ:
   CLI Entry → Config → Session → Provider → Tool → Agent → Plugin
5. จัดกลุ่มเป็นบท: แต่ละบท = กลุ่มแนวคิดที่ depend กัน
6. เรียงบทตาม dependency: สิ่งที่ไม่มี prerequisite → ก่อน
```

### Validation

```
□ ทุกบท i: concept ในบท i ไม่ depend on concept ในบท i+1 (หรือหลังกว่า)
□ บทแรกสอนสิ่งที่ไม่มี prerequisite
□ บทสุดท้ายใช้ทุกอย่างที่เรียนมา
□ กราฟไม่มี cycle (A→B→A)
```

---

## 4. Glossary / Pattern / Cheatsheet Outputs

นอกจากบทเรียน ต้องสร้างไฟล์เสริม:

### 4.1 `glossary.md`
```
ทุก chapter → เก็บคำศัพท์ที่ปรากฏครั้งแรก + อธิบายแบบคนไทยเข้าใจ

รูปแบบ:
### <คำศัพท์>
- **ความหมาย:** (อธิบายด้วยภาษาไทยง่ายๆ)
- **ปรากฏใน:** [[บทที่ X]]
- **ไฟล์ใน repo:** `path/to/file`
```

### 4.2 `patterns.md`
```
รวบรวม design patterns / architecture techniques จาก repo

รูปแบบ:
### <ชื่อ Pattern>
- **คืออะไร:** (1-2 บรรทัด)
- **ใช้ที่ไหนใน repo:** `path/to/file` → `function()`
- **ทำไมถึงใช้:** (เหตุผล)
- **เมื่อไหร่ควรใช้เอง:** (transfer)
- **บทที่สอน:** [[บทที่ X]]
```

### 4.3 `anti-patterns.md`
```
สิ่งที่ repo หลีกเลี่ยง หรือจุดที่ไม่ควรทำตาม

รูปแบบ:
### <ชื่อ Anti-Pattern>
- **คืออะไร:** (1-2 บรรทัด)
- **พบที่ไหน:** (ถ้ามี)
- **ทำไมไม่ควรทำ:** (เหตุผล)
- **ควรทำอะไรแทน:**
```

### 4.4 `cheatsheet.md`
```
ตารางตัดสินใจ / สรุปเร็ว — สำหรับคนอ่านจบแล้วกลับมาทบทวน

รูปแบบ:
| สถานการณ์ | ใช้แนวคิดอะไร | ดูบท |
|-----------|-------------|------|
| อยากทำ X | ใช้ Y | [[บทที่ Z]] |
```

### 4.5 `source-map.md`
```
แนวคิด ↔ ไฟล์จริง ↔ บทที่เกี่ยวข้อง

รูปแบบ:
| แนวคิด | ไฟล์ใน repo | บทที่สอน | ชั้น Knowledge |
|--------|-----------|---------|---------------|
| Session | packages/core/session/ | บท 3 | 1-5 |
```

---

## 5. Evaluation Rubric (ตรวจตัวเองก่อนส่ง)

ทุกบทต้อง self-score — **คะแนน < 85 ห้ามส่ง final**

### Textbook Rubric /100

| หมวด | คะแนน | ตรวจอะไร |
|------|--------|---------|
| **Accuracy** | 25 | ข้อมูลถูกต้อง `[✓]`, ไม่มี hallucination |
| **Source Traceability** | 20 | ทุก claim มี source, `[✓]` > 10 จุด |
| **Teaching Clarity** | 15 | อ่านแล้วเข้าใจ, ไม่ใช่ข้อมูลกอง |
| **Knowledge Density** | 15 | 5 ชั้นครบทุกหัวข้อหลัก |
| **Practical Transfer** | 15 | บอกได้ว่าเอาไปใช้กับโปรเจกต์ตัวเองยังไง |
| **Exercises Quality** | 10 | แบบฝึกหัด 3-5 ข้อ, มีเฉลย, ระดับต่างกัน |

### Scoring Process

```
1. เขียนบทเสร็จ
2. AI ตรวจตัวเอง:
   - Accuracy: ตรวจทุก claim → มี source ไหม?
   - Source Traceability: นับ `[✓]` → ต้อง ≥ 10
   - Teaching Clarity: อ่านซ้ำ → คนที่ไม่รู้มาก่อนเข้าใจไหม?
   - Knowledge Density: random 3 หัวข้อ → มีครบ 5 ชั้นไหม?
   - Practical Transfer: มี "เอาไปใช้ยังไง" ทุกหัวข้อหลัก?
   - Exercises: 3-5 ข้อ, มีทบทวน+วิเคราะห์+ลงมือทำ?

3. คำนวณคะแนนรวม
4. ถ้า < 85 → แก้ไข (revise) → ตรวจใหม่
5. ถ้า ≥ 85 → ส่ง final
```

### Quick Self-Check (ก่อน scoring จริง)

```
□ มี `[✓]` อย่างน้อย 10 จุด
□ ไม่มี `[!]` ในข้อความสำคัญ
□ 5 ชั้นครบทุกหัวข้อหลัก
□ Architecture diagram โยงถึงไฟล์ได้
□ แบบฝึกหัดมีเฉลย
□ มี "จุดที่มือใหม่มักเข้าใจผิด"
□ ภาษาไทยทั้งหมด (ยกเว้นชื่อ, code, URL)
```

---

## 6. Reader Persona (เลือกระดับก่อนเขียน)

| Persona | ระดับ | น้ำหนักการเขียน |
|---------|-------|---------------|
| **Beginner** | เพิ่งเริ่มเขียนโค้ด | เน้น concept + mechanism, อธิบายทุกอย่าง, มี example เยอะ |
| **Junior Dev** | เขียนโค้ดเป็น แต่ไม่รู้ architecture | เน้น mechanism + design reason, มี code walkthrough |
| **Intermediate** | รู้ architecture พื้นฐาน | เน้น design reason + tradeoff, มี comparison |
| **System Designer** | ออกแบบระบบเป็น | เน้น transfer + boundary + extension |
| **Maintainer** | จะ contribute repo นี้ | เน้น source evidence + code walkthrough + test |

**Rule:** 1 บท = 1 Persona — ถ้าจะเขียนให้หลายระดับ → แยกบท

---

## Code Reading Pipeline (7 ขั้น)

1. README.md → เป้าหมายโปรเจกต์
2. package/config → stack, dependencies
3. entrypoint → จุดเริ่มระบบ
4. core modules → แกนหลัก
5. adapters/integrations → การเชื่อมต่อภายนอก
6. tests/examples → intended usage
7. docs/issues/releases → บริบทและข้อจำกัด

⚠️ **ห้ามเขียน Architecture Chapter จาก README อย่างเดียว** — README คือขั้นที่ 1 จาก 7

---

## Chapter Types (6 แบบ)

| Type | ใช้เมื่อ |
|------|--------|
| 🧠 **Concept** | สอนแนวคิดหลัก |
| 🏗️ **Architecture** | สอนภาพรวมระบบ |
| 🔬 **Code Reading** | พาเดินอ่านไฟล์จริง |
| 🔨 **Implementation** | สร้าง mini version |
| ⚖️ **Comparison** | เทียบ design choice |
| ❓ **Exercise** | ทดสอบความเข้าใจ |

---

## Pass Levels

| Level | Sections | ใช้เมื่อ |
|-------|----------|---------|
| **Quick Note** | 1-3 | repo เล็ก, preview |
| **Standard Lesson** | 5-8 | concept/module เดียว |
| **Full Textbook** | 10-15 | repo ใหญ่, foundation |

### Artifact Budget (ไฟล์สูงสุดต่อ pass)

| Pass Level | สูงสุด | ถ้าเกิน |
|-----------|--------|--------|
| **Quick Note** | 1 ไฟล์ | ถ้าจะเกิน → ทำไมต้องเกิน? หรือตัดสินใจผิดตั้งแต่ intake? |
| **Standard Lesson** | 3-5 ไฟล์ (บท + glossary + patterns) | ถ้าจะเกิน → split เป็นหลาย pass |
| **Full Textbook** | 8-10 ไฟล์ (book-plan + บท + glossary + patterns + cheatsheet + source-map) | ถ้าจะเกิน → ระบุ trigger + evidence ก่อนสร้างเพิ่ม |

**Promotion Gate (ข้าม Pass Level):**
- Name the trigger: "พบ 3+ modules ที่เกี่ยวเนื่องกัน"
- Cite evidence: user request หรือ inspected complexity
- State risk of staying smaller: "ถ้าไม่ทำ Full — missing cross-module patterns"

**หากไม่แน่ใจ:** อยู่ใน pass ที่ต่ำกว่า, inspect แคบๆ ก่อน, ไม่ promote เพียงเพราะ "อาจจะต้อง"

---

## Evidence Strength

| เครื่องหมาย | ระดับ | ใช้เมื่อ |
|-----------|------|---------|
| `[✓]` | Direct | ยืนยันจาก repo/docs โดยตรง |
| `[→]` | Inferred | อนุมานจากหลักฐาน |
| `[?]` | Assumed | ตั้งสมมติฐาน |
| `[!]` | Unverified | ยังไม่มีหลักฐาน |

---

## Self-Score Rubric Quick Reference

```
□ Accuracy:          /25  (ทุก claim มี source?)
□ Source Traceability: /20  ([✓] ≥ 10 จุด?)
□ Teaching Clarity:   /15  (มือใหม่อ่านรู้เรื่อง?)
□ Knowledge Density:  /15  (5 ชั้นครบ?)
□ Practical Transfer: /15  (เอาไปใช้ได้?)
□ Exercises Quality:  /10  (3-5 ข้อ, มีเฉลย?)

Total: __/100  →  □ ≥ 85 ส่ง  □ < 85 revise
```

---

---

## Report Structure (สรุปหลังจบทุกโปรเจค)

หลังเขียนทุกบทเสร็จ ต้องรายงาน:

```
1. What was inspected — repo, files, docs
2. Selected pass level — Quick / Standard / Full + why
3. Book-plan — target reader, chapter count, dependency graph
4. Chapters created — list + chapter type + self-score
5. Evidence summary — [✓] count, [→] count, [!] items
6. Unknowns — inspection limitations, questions not answered
7. Risks — unclear boundaries, missing data
8. Glossary/Cheatsheet — created or not + why
9. Recommended next — what to study next, in vault links
```

---

## Output Files Checklist

ก่อนจบโปรเจค ตรวจสอบว่ามีไฟล์เหล่านี้ตามความเหมาะสม:

```
□ book-plan.md          # แผนทั้งเล่ม (required)
□ chapter-*.md          # บทเรียน (required)
□ glossary.md           # คำศัพท์ (recommended)
□ patterns.md           # design patterns (recommended)
□ anti-patterns.md      # สิ่งที่ไม่ควรทำ (optional)
□ cheatsheet.md         # ตารางสรุปเร็ว (optional)
□ source-map.md         # แนวคิด↔ไฟล์↔บท (optional)
```

---

## Templates

- `templates/concept-chapter.md`
- `templates/architecture-chapter.md`
- `templates/code-reading-chapter.md`
- `templates/implementation-chapter.md`
- `templates/comparison-chapter.md`
- `templates/exercise-chapter.md`
- `templates/book-plan.md`

## Checklists

- `checklists/repo-inspection.md`
- `checklists/evidence-check.md`
- `checklists/textbook-quality.md`
- `checklists/evaluation-rubric.md`
