# Changelog — Safety Passport

## 2026-05-30 (Major update)

### ⭐ New
- **download-cert.html** — vendor สามารถดาวน์โหลด cert ของตัวเองโดยไม่ต้อง login ได้
- **Admin panel** ครบ 6 หน้า — Login + Dashboard + Requests + Detail + Calendar + Cert Lookup
- **Admin Actions** — Approve / Reschedule / Cancel จาก admin panel ตรงๆ (พร้อมส่ง email vendor)

### 🎨 Design
- โลโก้ ปตท. จริงทุกหน้า (sidebar admin + header vendor + login)
- Login admin: 2-column layout บน desktop / stack บน mobile
- Request-detail: เพิ่มแถว "วันนัดอบรม" พร้อม highlight ถ้าเปลี่ยนจากวันที่ vendor ขอ
- Responsive ทั้ง desktop + mobile

### 🔧 Fixes
- เกณฑ์สอบ: `7/10` → `8/10` (start.html)
- ลบปุ่ม "เริ่มกรอกฟอร์ม" ที่ซ้ำกับ Step 1
- เปลี่ยน `ปกต.` → `ปท.` ทั่วทั้งระบบ

### 🌐 URLs
- Repo: `safety-passport`
- Live: https://pttosp1-wq.github.io/safety-passport/
