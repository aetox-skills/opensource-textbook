# Repo-to-Textbook Skill

> สกิลสำหรับแปลง Open Source repository → ตำราเรียนภาษาไทย

## ใช้ทำอะไร

ให้ AI (OpenCode, Claude Code, Codex) อ่าน Open Source repository แล้วเขียนเป็นบทเรียนภาษาไทย — จากไม่รู้ → เข้าใจ → วิเคราะห์ได้ → ลงมือทำได้

## เหมาะกับใคร

- คนที่อยากเรียนสถาปัตยกรรมจาก Open Source จริง
- คนที่อยากให้ AI สอนแทนการสรุปเฉยๆ
- นักพัฒนาไทยที่อยากอ่านตำราภาษาไทย

## วิธีการติดตั้ง

```bash
# ใน OpenCode — เปิด project → พิมพ์
/skills add aetox-skills/opensource-textbook
```

หรือเพิ่มใน `opencode.json`:

```json
{
  "skills": {
    "urls": ["https://github.com/aetox-skills/opensource-textbook"]
  }
}
```

## ตัวอย่างการใช้งาน

```
# ตัวอย่าง prompt
ใช้ opensource-textbook skill
ทำบทเรียนจาก https://github.com/anomalyco/opencode
เป็น Architecture Chapter ภาษาไทย
```

## ตัวอย่าง Output

```
📘 เอกสารอธิบายระบบ OpenCode — Technical Documentation
├── 1. ภาพรวมระบบ
├── 2. สถาปัตยกรรม 3 ชั้น (diagram)
├── 3. โครงสร้าง Monorepo (22 packages)
├── 4. Dual-Agent Architecture
├── 5. Session Management
├── 6. Provider Abstraction Layer
├── ...
└── 15. คำถามท้ายบท
```

## ความสามารถหลัก

| ความสามารถ | รายละเอียด |
|-----------|-----------|
| **Code Reading Pipeline** | 7 ขั้น — ห้ามข้าม, ห้ามเขียน arch จาก README อย่างเดียว |
| **Evidence Strength** | `[✓]` Direct, `[→]` Inferred, `[?]` Assumed, `[!]` Unverified |
| **6 Chapter Types** | Concept, Architecture, Code Reading, Implementation, Comparison, Exercise |
| **Pass Levels** | Quick Note → Standard → Full Textbook |
| **Checkpoint Gates** | 6 gates — ตรวจก่อน publish |

## ข้อจำกัด

- ต้องให้ AI อ่าน repo จริงก่อน (ใช้ webfetch/clone)
- ไม่เหมาะกับ repo ที่ไม่มี README หรือ documentation เลย
- ภาษาไทยเท่านั้น — ไม่รองรับภาษาอื่น

## โครงสร้าง

```
opensource-textbook/
├── SKILL.md           ← contract สำหรับ AI
├── README.md          ← คู่มือมนุษย์ (ไฟล์นี้)
├── templates/         ← 6 templates บทเรียน
└── checklists/        ← 3 checklists ตรวจคุณภาพ
```

## License

MIT — ใช้ได้อิสระ
