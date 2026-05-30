# 🧪 TEST CHECKLIST — PTT TSO Safety Training System
## ก่อน Go-Live

**Date:** 2026-05-25
**Tester:** จิรายุ สุลัยหมัด (PO7)
**System URL:** https://pttosp1-wq.github.io/ptt-cert/

---

## 📋 Pre-Flight Checks

### ✅ ก่อนเริ่มเทสต์ ทุกข้อต้องผ่าน

- [ ] Flow 1 (CheckTraining_PO7) — Status: **On**
- [ ] Flow 2 (SaveExam_GenerateCert_PO7) — Status: **On**
- [ ] Flow 3 (SubmitRequest_PKT2) — Status: **On**
- [ ] Flow 4 (ConfirmRequest_PKT2) — Status: **On**
- [ ] HTML ใหม่ทุกไฟล์ upload ขึ้น GitHub Pages แล้ว
- [ ] Hard refresh ทุกหน้า (Ctrl+Shift+R) หรือใช้ Incognito
- [ ] ContractorData.xlsx เปิดได้ — ทุก column = **Text format**

---

## 🗂️ Round 1: เตรียม Excel Test Data (10 นาที)

### Sheet: `Requests` — เตรียม 3 rows

> ⚠️ **ScheduledDate ใน Row 1, 2 = วันที่จะเทสต์** (ถ้าเทสต์ 25 พ.ค. ใส่ `2026-05-25`)

| Row | RequestID | ScheduledDate | Status | จุดประสงค์ |
|---|---|---|---|---|
| 1 | REQ-260520-0001 | **วันนี้** | Approved | สำหรับเทสต์ NID `1111111111111` (valid cert) |
| 2 | REQ-260520-0002 | **วันนี้** | Approved | สำหรับเทสต์ NID `2222222222222` (expired) |
| 3 | REQ-260520-0003 | **อนาคต** (30 พ.ค.) | Approved | สำหรับเทสต์ NID `3333333333333` (wrong_date) |

**Fields อื่นๆ ใส่ตามนี้** (กรอกเหมือนกันทั้ง 3 rows ก็ได้):
- SubmittedDate: `2026-05-20`
- Project: `Test Project`
- Company: `TestCo`
- ContactName: `ทดสอบ`
- ContactPhone: `0810000000`
- ContactEmail: email ของตัวเอง
- Division: `PO7`
- WorkingArea: `Test Area`
- WorkStart/WorkEnd: `2026-06-01` / `2026-06-30`
- PttEngName: `จิรายุ สุลัยหมัด`
- PttEngEmail: `jirayu.s@pttplc.com`
- ScheduledTime: `09:00`
- PreferredTrainingDate/Time: เหมือน ScheduledDate/Time

- [ ] Requests sheet มี 3 rows
- [ ] ScheduledDate ของ row 1, 2 = วันที่เทสต์
- [ ] Status ทุก row = `Approved`

### Sheet: `Trainees` — เตรียม 4 rows

| RequestID | FullName | NationalID | IDType | Status |
|---|---|---|---|---|
| REQ-260520-0001 | ทดสอบ มีเซอร์ | **1111111111111** | ThaiID | Approved |
| REQ-260520-0002 | ทดสอบ เซอร์หมด | **2222222222222** | ThaiID | Approved |
| REQ-260520-0003 | ทดสอบ ผิดวัน | **3333333333333** | ThaiID | Approved |
| REQ-260520-0001 | Test Passport | **AB123456** | Passport | Approved |

**Fields อื่น:** Prefix=`นาย`, Position=`Test`, Division=`PO7`
**TraineeID** = `{RequestID}_{NationalID}` เช่น `REQ-260520-0001_1111111111111`

- [ ] Trainees sheet มี 4 rows
- [ ] NID 13 หลัก format Text ไม่เป็น Scientific Notation

### Sheet: `TrainingData` — เตรียม 2 rows

| FullName | NationalID | IsPassed | ExpiryDate | CertNo |
|---|---|---|---|---|
| ทดสอบ มีเซอร์ | **1111111111111** | TRUE | **2026-08-15** (อนาคต) | PO7-250815-1111 |
| ทดสอบ เซอร์หมด | **2222222222222** | TRUE | **2025-01-10** (อดีต) | PO7-240110-2222 |

**Fields อื่น:** Company=`TestCo`, Score=`9`, ExamDate=`2025-08-15`/`2024-01-10`, CertLink=URL ใดก็ได้, Division=`PO7`

- [ ] TrainingData มี 2 rows
- [ ] ExpiryDate ของ row 1 ยังไม่หมด (อนาคต) → จะได้ state `valid`
- [ ] ExpiryDate ของ row 2 หมดแล้ว (อดีต) → จะได้ state `expired`

---

## 🟢 Round 2: Phase 1 — Vendor Submission (15 นาที)

### TC1.1 — Select Division
**URL:** https://pttosp1-wq.github.io/ptt-cert/select-division.html

- [ ] เห็นการ์ด 4 ใบ: PO5, PO6, PO7, PO8
- [ ] กด **PO7** → เด้งไป `start.html?division=PO7`
- [ ] header แสดง "PO7"

### TC1.2 — Start Page
- [ ] เห็นกติกา 5 ข้อ
- [ ] กด "เริ่มส่งคำขอ" → เด้งไป `request.html?division=PO7`
- [ ] header tag แสดง "PO7"

### TC1.3 — Custom Calendar
- [ ] Calendar แสดงเดือนปัจจุบัน + ปี **พ.ศ.** (เช่น "พฤษภาคม 2569")
- [ ] เสาร์-อาทิตย์ = แดง + คลิกไม่ได้
- [ ] วันหยุดราชการ (เช่น 3 พ.ค. = วันฉัตรมงคล) = ส้ม + คลิกไม่ได้
- [ ] วันที่ผ่านมาแล้ว = เทา + คลิกไม่ได้
- [ ] กดวันธรรมดาในอนาคต → banner ฟ้าโชว์วันที่ + เวลา 09:00
- [ ] กด ‹ › เปลี่ยนเดือนได้

### TC1.4 — Form Validation
- [ ] กดส่งโดยไม่กรอกอะไร → alert "กรอกข้อมูลให้ครบ"
- [ ] กรอก email มั่ว (เช่น `abc`) → alert "รูปแบบ Email ไม่ถูกต้อง"
- [ ] ไม่เลือกวัน → alert "เลือกวันที่ต้องการอบรม"
- [ ] Trainee ใส่ NID 12 หลัก → alert validation
- [ ] Trainee ใส่ Passport `AB12` (4 ตัว) → alert validation
- [ ] Trainee ใส่ Passport `AB1234` (6 ตัว) → ✅ pass
- [ ] Trainee ไม่ใส่ตำแหน่ง → alert "ตำแหน่ง"

### TC1.5 — Add/Remove Trainees + UI
- [ ] กด "+ เพิ่มรายชื่อผู้เข้าอบรม" → row ใหม่ขึ้นมา
- [ ] Row 2+ มีปุ่ม "✕ ลบ" — hover เปลี่ยนเป็นแดง
- [ ] กดลบ row กลางๆ → "คนที่ N" renumber อัตโนมัติ
- [ ] ช่อง input ใหญ่ + มน (ไม่เล็กเหลี่ยม)

### TC1.6 — ⭐ Submit สำเร็จ (Flow 3)

**ข้อมูลทดสอบ:**
```
โครงการ        : ทดสอบ Production Go-Live
รายละเอียดงาน  : ทดสอบการส่งคำขออบรม
สถานที่        : Test Station
วันเริ่มงาน    : 2026-06-15
วันสิ้นสุดงาน  : 2026-06-30
วันที่อบรม     : เลือกวันธรรมดาในอนาคต
เจ้าหน้าที่ ปตท. : จิรายุ สุลัยหมัด

ผู้ติดต่อ:
  บริษัท              : Test Production Co., Ltd.
  ชื่อ-นามสกุลผู้ติดต่อ : สมชาย จัดการดี
  เบอร์มือถือ          : 0812345678
  Email               : jirayu.s@pttplc.com  ← ใส่ email ตัวเอง

ผู้เข้าอบรม:
  คนที่ 1: นาย ทดสอบ คนแรก / NID 4444444444444 / ตำแหน่ง ช่างเชื่อม
  คนที่ 2: นางสาว ทดสอบ Passport / XY789012 / ตำแหน่ง วิศวกร
```

**คาดหวัง:**
- [ ] Modal "กำลังส่งคำขอ..." → "✅ ส่งคำขอเรียบร้อย"
- [ ] แสดง RequestID (จดไว้: `REQ-________________`)
- [ ] Email มาถึง `jirayu.s@pttplc.com` พร้อม Magic Link
- [ ] Email subject มีชื่อ "Test Production Co., Ltd. - สมชาย จัดการดี" (combine)
- [ ] Excel Requests: row ใหม่ Status = `Pending`
- [ ] Excel Trainees: 2 rows ใหม่ Status = `Pending` (Passport `XY789012` ตัวใหญ่)

### TC1.7 — Error Handling
- [ ] ปิด WiFi → กดส่ง → Modal error + ปุ่ม submit re-enable
- [ ] (Optional) Turn off Flow 3 → กดส่ง → Modal error

---

## 🔵 Round 3: Phase 2 — จป. Confirmation (10 นาที)

### TC2.1 — Magic Link Confirm (วันเดิม)
**URL:** จาก email Magic Link ที่ได้จาก TC1.6

- [ ] กด link "ยืนยันวันอบรม" → เปิด confirmed.html
- [ ] เห็น State loading → success
- [ ] แสดงวันแบบไทย (เช่น "พฤหัสบดี 5 มิ.ย. 2569 เวลา 09:00 น.")
- [ ] Excel Requests row ของ RequestID: Status → `Approved`
- [ ] Excel Trainees rows: Status → `Approved`
- [ ] Email "ยืนยันวันอบรม" ส่งถึง vendor email

### TC2.2 — Pick-Date (เปลี่ยนวัน)
**URL:** จาก email มี link "เปลี่ยนวัน"

- [ ] กด link "เปลี่ยนวัน" → pick-date.html
- [ ] Header แสดง RequestID ที่ถูกต้อง
- [ ] Calendar คลิกเสาร์-อาทิตย์ ได้ (จป. มีอำนาจ)
- [ ] คลิกวันใหม่ + เลือกเวลา 13:00 → summary card แสดงสรุป
- [ ] กด "ยืนยันวันใหม่" → redirect ไป confirmed.html → success
- [ ] Excel: ScheduledDate, ScheduledTime อัพเดทเป็นค่าใหม่

### TC2.3 — confirmed.html Error
- [ ] เปิด URL `confirmed.html` ตรงๆ (ไม่มี params) → State error
- [ ] ใส่ RequestID มั่ว → State error + ปุ่ม retry

---

## 🟠 Round 4: Phase 3 — Exam Day (20 นาที)

### TC3.1 — index.html — 5 States
**URL:** https://pttosp1-wq.github.io/ptt-cert/index.html (Division = PO7)

| Test | NID/Passport | คาดหวัง State |
|---|---|---|
| 3.1.A | `9999999999999` | ❌ **not_registered** (block) |
| 3.1.B | `3333333333333` | 📅 **wrong_date** (แสดงวันจริง 30 พ.ค.) |
| 3.1.C | `1111111111111` | ✅ **valid** (แสดง cert + ปุ่ม PDF) |
| 3.1.D | `2222222222222` | ⚠️ **expired** (warning + ปุ่มสอบใหม่) |
| 3.1.E | `4444444444444` | ➡️ **not_found** (ปุ่มเริ่มสอบ) |
| 3.1.F | `XY789012` (Passport) | ➡️ **not_found** |

- [ ] 3.1.A ผ่าน (not_registered)
- [ ] 3.1.B ผ่าน (wrong_date + แสดงวันจริง)
- [ ] 3.1.C ผ่าน (valid + cert details)
- [ ] 3.1.D ผ่าน (expired)
- [ ] 3.1.E ผ่าน (not_found, NID ใหม่จาก TC1.6)
- [ ] 3.1.F ผ่าน (Passport auto-uppercase ทำงาน)

### TC3.2 — exam.html Pre-Validation
- [ ] จาก index.html state `not_found` → กด "เริ่มทำแบบทดสอบ" → exam.html
- [ ] exam.html ขึ้น screen "validating" สั้นๆ
- [ ] เปลี่ยนเป็น screen "intro" (กติกา 5 ข้อ)
- [ ] ลองเปิด exam.html ตรงๆ ด้วย NID `3333333333333` (wrong_date) — Pre-validation block ได้
- [ ] ลองเปิด exam.html ด้วย NID `9999999999999` — Pre-validation block ได้ (not_registered)
- [ ] ลองเปิด exam.html ด้วย NID `1111111111111` (valid) → screen "has-valid-cert" + ปุ่ม "สอบใหม่เพื่อต่ออายุ"

### TC3.3 — Exam Flow (ทำข้อสอบ)
- [ ] Progress bar แสดง "ข้อ 1 / 10"
- [ ] ข้อสอบสุ่มจาก 20 (ลองทำ 2 ครั้ง — เห็นข้อต่างกัน)
- [ ] ตัวเลือกในแต่ละข้อสุ่มลำดับ
- [ ] กดตัวเลือก → highlight ฟ้า + ปุ่ม "ถัดไป" enable
- [ ] กด "← ย้อนกลับ" → คำตอบเดิมยังอยู่
- [ ] ข้อสุดท้ายปุ่มเปลี่ยนเป็น "ส่งคำตอบ ✓"

### TC3.4 — ⭐⭐⭐ สอบผ่าน (8+/10) — สำคัญที่สุด
**ใช้ NID `4444444444444`** (จาก TC1.6) — เลือก choice ที่ถูก (ปกติ "หยุดงาน + แจ้งหัวหน้า")

- [ ] คะแนน ≥ 8/10
- [ ] Screen "submitting" → "กำลังออกใบวุฒิบัตร..." → "กำลังบันทึกผลสอบ..."
- [ ] Screen "pass" แสดงคะแนน
- [ ] CertNo format `PO7-YYMMDD-4444` (4 หลักท้าย NID)
- [ ] กด "📄 ดาวน์โหลดใบวุฒิบัตร" → PDF เปิด/โหลด
- [ ] PDF แสดง: ชื่อ + examDate + expiryDate (สีแดง) + certNo
- [ ] ⭐ Excel TrainingData row ใหม่: **CertNo ใน Excel = CertNo บน PDF = CertNo บนหน้าจอ** (3 ที่ตรงกัน!)
- [ ] OneDrive มีไฟล์ `{CertNo}.pdf`

### TC3.5 — สอบไม่ผ่าน (<8/10)
**ใช้ trainee ใหม่ (สร้าง request อีกอันหรือใช้ Passport `XY789012`)** — เลือก choice ผิดๆ

- [ ] คะแนน < 8/10
- [ ] Screen "fail" แสดงคะแนน + "เกณฑ์ผ่าน: 8 ข้อขึ้นไป"
- [ ] กด "ลองใหม่" → กลับไป exam state พร้อมข้อสุ่มใหม่
- [ ] Excel TrainingData **ไม่มี** row ใหม่
- [ ] OneDrive **ไม่มี** ไฟล์ PDF

### TC3.6 — Cert Renewal (สอบใหม่ทั้งที่ยังมี cert)
**ใช้ NID `1111111111111`** (มี cert ยังไม่หมดอายุ)

- [ ] index.html → state `valid` → แสดง cert เดิม
- [ ] เปิด exam.html ตรงๆ ด้วย NID นี้ → screen "has-valid-cert"
- [ ] กดปุ่ม "สอบใหม่เพื่อต่ออายุ" → เข้าสู่ exam ได้
- [ ] สอบผ่าน → cert ใหม่ + Excel TrainingData มี row ใหม่ (NID เดิมแต่ CertNo + ExpiryDate ใหม่)

---

## 🔴 Round 5: Security Tests (10 นาที)

### TC4.1 — ⭐ Flow 2 Security Guard

**วิธี:** เปิด DevTools (F12) → Console tab → วาง code:

```javascript
// Test 1: NID มั่ว (not_registered) → ต้อง DENY
fetch('https://defaultf2fda5e72ea1450d9fc12af5f86300.95.environment.api.powerplatform.com:443/powerautomate/automations/direct/workflows/8394a34120814821add245c7376e40f5/triggers/manual/paths/invoke?api-version=1&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=UORTFcL-JLw_zlRE0Kv8TN8Yq4nxPYsC2ugT05PxAxk', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    apiKey: 'PTT-PO7-2026-SECRET',
    nationalId: '9999999999999',
    fullName: 'Hacker Test',
    company: 'Evil Corp',
    division: 'PO7',
    score: 10, isPassed: true, examDate: '2026-05-25',
    certNo: 'PO7-260525-9999',
    expiryDate: '2027-05-25',
    pdfBase64: 'JVBERi0xLjMK'
  })
}).then(r => r.json()).then(d => console.log('Test 1 (DENY):', d));
```

- [ ] Response: `{ status: "not_authorized", ... }`
- [ ] **ไม่มี** Excel row ใหม่ใน TrainingData
- [ ] **ไม่มี** ไฟล์ PDF ใน OneDrive

```javascript
// Test 2: NID ผิดวัน (wrong_date) → ต้อง DENY
fetch('https://defaultf2fda5e72ea1450d9fc12af5f86300.95.environment.api.powerplatform.com:443/powerautomate/automations/direct/workflows/8394a34120814821add245c7376e40f5/triggers/manual/paths/invoke?api-version=1&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=UORTFcL-JLw_zlRE0Kv8TN8Yq4nxPYsC2ugT05PxAxk', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    apiKey: 'PTT-PO7-2026-SECRET',
    nationalId: '3333333333333',
    fullName: 'Wrong Date User',
    company: 'TestCo',
    division: 'PO7',
    score: 10, isPassed: true, examDate: '2026-05-25',
    certNo: 'PO7-260525-3333',
    expiryDate: '2027-05-25',
    pdfBase64: 'JVBERi0xLjMK'
  })
}).then(r => r.json()).then(d => console.log('Test 2 (DENY wrong_date):', d));
```

- [ ] Response: `{ status: "not_authorized", ... }`

### TC4.2 — Security Mapping (เช็คให้ครบ)

| nationalId | Flow 1 จะตอบ | Flow 2 ต้องเป็น |
|---|---|---|
| `9999999999999` | not_registered | ❌ 403 not_authorized |
| `3333333333333` | wrong_date | ❌ 403 not_authorized |
| `4444444444444` | not_found | ✅ ALLOW |
| `2222222222222` | expired | ✅ ALLOW |
| `1111111111111` | valid | ✅ ALLOW (renew) |

- [ ] Mapping ครบทั้ง 5 cases

---

## 🟣 Round 6: Edge Cases (15 นาที)

### TC5.1 — TraineeID ซ้ำ NID
ส่ง request ใหม่ที่มี NID `4444444444444` ที่เคยใช้ใน TC1.6:
- [ ] Trainees sheet มี 2 rows ที่มี NID ซ้ำ แต่ TraineeID ต่างกัน (RequestID ต่าง)

### TC5.2 — Passport Support
- [ ] กรอก `ab12345` (lowercase 7 ตัว) → auto-uppercase เป็น `AB12345`
- [ ] กรอก `AB-123 456` (มี dash + space) → strip เป็น `AB123456`
- [ ] กรอก `XY1` (3 ตัว) → ปุ่ม disabled
- [ ] Submit Passport user → CertNo ใช้ 4 หลักท้าย เช่น `PO7-260525-3456`

### TC5.3 — Mobile Responsive
**เปิดบนมือถือ** (หรือ DevTools → Toggle device → iPhone 12)
- [ ] select-division: การ์ด 4 ใบเรียงสวย
- [ ] request.html: form fields ใหญ่พอ tap ได้
- [ ] calendar แสดงครบทุกวัน
- [ ] exam.html: ตัวเลือกข้อสอบ tap ได้สะดวก

### TC5.4 — Browser Compatibility
- [ ] Chrome (latest) ✓
- [ ] Safari (มือถือ iOS) ✓
- [ ] Edge ✓

### TC5.5 — localStorage Persistence
- [ ] กรอก index.html → กด "เริ่มทดสอบ" → เปิด exam.html
- [ ] เปิด DevTools → Application → Local Storage
- [ ] มี: `ptt_nationalId`, `ptt_division`, `ptt_fullName`, `ptt_company`
- [ ] Reload exam.html (F5) → ระบบยังจำได้ (ไม่ต้องกรอก NID ใหม่)

### TC5.6 — Concurrent Submissions
- [ ] เปิด request.html **2 tabs** พร้อมกัน → กรอกข้อมูลคนละชุด → กดส่งทั้งคู่ในเวลาใกล้กัน
- [ ] ได้ RequestID คนละเลข
- [ ] Excel มี 2 rows ใหม่ ไม่ซ้ำกัน

### TC5.7 — Special Characters
- [ ] ทดสอบชื่อ: ภาษาไทยมี space "สมชาย ใจดี" → บันทึก Excel ได้
- [ ] ทดสอบชื่ออังกฤษ "John Smith" → บันทึก Excel ได้
- [ ] ชื่อมีตัว space หลายช่อง → trim ทำงาน

---

## ⚙️ Round 7: Flow Configuration Checks (5 นาที)

### TC6.1 — Flow 2 Condition ใช้ boolean
**วิธี:** เปิด Power Automate → Flow 2 → คลิก Condition action ที่เช็คผ่าน/ไม่ผ่าน

- [ ] Expression: `equals(triggerBody()?['isPassed'], true)` (boolean) ✓
- [ ] ❌ ไม่ใช่ `equals(triggerBody()?['isPassed'], 'yes')` (string)

### TC6.2 — Flow 2 Security Guard Actions
**วิธี:** เปิด Flow 2 → ดู actions ก่อน Condition

- [ ] มี HTTP action ชื่อ `HTTP_Call_Flow_1` (เรียก Flow 1)
- [ ] มี Condition ชื่อ `Authorize_Condition` (เช็ค status)
- [ ] If False branch มี Response action ส่ง `{status: "not_authorized", ...}`
- [ ] If False branch มี Terminate action (Cancelled)

### TC6.3 — Flow 2 CertNo binding
**วิธี:** เปิด Flow 2 → `Add a row into a table_1` action

- [ ] `item/CertNo = @triggerBody()?['certNo']` (จาก HTML)
- [ ] ❌ ไม่ใช่ `concat(... rand(1000, 9999))` (สร้างใหม่)

### TC6.4 — Excel Text Format
- [ ] ContractorData.xlsx ทุก column = Text format
- [ ] NID 13 หลักไม่แปลงเป็น Scientific (`1.23457E+12`)
- [ ] วันที่เก็บเป็น string `2026-05-25` ไม่ใช่ serial `45798`

---

## 🎯 Priority Tests (ถ้าเวลาน้อย)

### ⭐ ต้องผ่านก่อน Go-Live (12 cases)

1. [ ] **TC1.6** — Submit คำขอ + ได้ RequestID + email
2. [ ] **TC2.1** — Magic Link confirm สำเร็จ
3. [ ] **TC3.1.A** — index.html block not_registered
4. [ ] **TC3.1.B** — index.html block wrong_date
5. [ ] **TC3.1.C** — index.html แสดง valid cert
6. [ ] **TC3.1.D** — index.html แสดง expired
7. [ ] **TC3.1.E** — index.html ส่งเข้า exam ได้
8. [ ] **TC3.4** — ⭐ สอบผ่าน + CertNo ตรงกัน 3 ที่
9. [ ] **TC3.5** — สอบไม่ผ่าน + ไม่มี row ใน Excel
10. [ ] **TC4.1** — ⭐ Security Guard ผ่าน DevTools test
11. [ ] **TC6.1** — Flow 2 Condition boolean
12. [ ] **TC6.4** — Excel Text format

---

## 🐛 Known Issues / Notes

### ⚠️ Flow 3 Excel Mapping (รู้แล้ว, ทำงานได้, แต่ไม่ optimal)

Flow 3 ปัจจุบันใช้ `vendor.name` แมพไปทั้ง 2 column ใน Excel:
- Excel Column **Company** = combined string `"บริษัท ABC - สมชาย ใจดี"`
- Excel Column **ContactName** = combined string เดียวกัน

ถ้าอยากแยกใน Excel อนาคต — ต้องแก้ Flow 3 mapping:
```
item/Company     = @triggerBody()?['vendor']?['company']
item/ContactName = @triggerBody()?['vendor']?['contactName']
```

HTML ส่ง `vendor.company` และ `vendor.contactName` มาให้แล้ว — รอ Flow update

### ⚠️ CERT_BG_BASE64 Placeholder

exam.html ปัจจุบันใช้ fallback background (พื้นหลังขาว + กรอบ navy) ถ้าต้องการ background ใบเซอร์สวยๆ:
1. เปิด `exam.html` ตัวเก่าใน GitHub (v1)
2. หาบรรทัด `const CERT_BG_BASE64 = '/9j/4AAQ...';`
3. Copy ทั้งสตริงมาวางที่ `const CERT_BG_BASE64 = '';` ในไฟล์ใหม่

---

## 📝 Tester Notes

**สิ่งที่ต้องทำหลังเทสต์เสร็จ:**
- [ ] Document ผลเทสต์ทุก case
- [ ] Screenshots ของ bug ที่เจอ (ถ้ามี)
- [ ] List of bugs to fix ก่อน go-live
- [ ] Sign-off ถ้าผ่านทุก Priority Tests

**Sign-off:**

```
Tester:      _______________________      Date: ___/___/______
PO7 Lead:    จิรายุ สุลัยหมัด              Date: ___/___/______
Status:      [ ] PASS  [ ] FAIL  [ ] PASS WITH CONDITIONS
```
