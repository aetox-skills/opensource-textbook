# 🗺️ Source Map Template

> แนวคิด ↔ ไฟล์จริง ↔ บทที่สอน — ทำให้ตำราไม่หลุดจาก codebase

---

| แนวคิด | บทที่สอน | Source File | Function/Class | Evidence |
|--------|---------|-------------|---------------|----------|
| Session Management | [[บทที่ 3]] | `src/session.ts` | `createSession()` | `[✓]` |
| Provider Abstraction | [[บทที่ 5]] | `src/provider/index.ts` | `Provider` interface | `[✓]` |
| Agent Loop | [[บทที่ 7]] | `src/agent/loop.ts` | `runLoop()` | `[→]` |
| ... | ... | ... | ... | ... |

---

## Usage

- **ผู้เรียน:** อยากดูโค้ดของแนวคิด X → ดูตาราง → ไปไฟล์นั้น
- **ผู้สอน:** ตรวจว่าทุกแนวคิดสำคัญมี source จริง → ไม่มีไฟล์ = `[!]`
- **AI ตัวต่อไป:** ใช้เป็น index สำหรับ navigate codebase
