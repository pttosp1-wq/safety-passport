วิธี Upload — safety-passport
==============================

ก่อนอื่น: ลบไฟล์ขยะ 3 ตัวออกจาก GitHub ก่อน
(เกิดจาก upload ผิดชื่อ/ผิดที่ ไฟล์จริงเลยไม่ถูกแตะ)

  https://github.com/pttosp1-wq/safety-passport/delete/main/admin-index.html
  https://github.com/pttosp1-wq/safety-passport/delete/main/request-detail.html
  https://github.com/pttosp1-wq/safety-passport/delete/main/admin/admin-index.html

แต่ละลิงก์ -> เลื่อนลงล่าง -> Commit changes


จากนั้น Upload ชุดใหม่
----------------------
1. Unzip ไฟล์นี้
2. ไป https://github.com/pttosp1-wq/safety-passport
3. Add file -> Upload files
4. ลากทุกอย่างที่ unzip ลงไป (รวมโฟลเดอร์ admin/ ด้วย)
   *** ห้ามลากตัวโฟลเดอร์ safety-passport ทั้งอัน ***
   *** ให้เข้าไปข้างในแล้วเลือกทุกอย่างข้างใน (Ctrl+A) ***
5. เลื่อนลงล่าง -> Commit changes


โครงสร้างที่ถูกต้อง
-------------------
รากของ repo/
  index.html
  start.html
  select-division.html
  pick-date.html
  exam.html               <- แก้แล้ว: ยามกันใบเปล่า
  request.html            <- แก้แล้ว: validation เลขบัตรซ้ำ
  confirmed.html
  download-cert.html
  README.md
  CHANGELOG.md
  TEST_CHECKLIST.md
  admin/
    login.html
    index.html            <- แก้แล้ว: field mapping
    requests.html
    request-detail.html   <- แก้แล้ว: field mapping
    calendar.html
    cert-lookup.html

ต้องไม่มี admin-index.html หรือ request-detail.html อยู่ที่รากของ repo


สิ่งที่แก้ในชุดนี้
------------------
1. admin/request-detail.html
   Flow ส่ง field ตัวเล็ก (fullName, nid) แต่โค้ดเรียกตัวใหญ่ (t.FullName)
   -> ทำให้รับได้ทั้งสองแบบด้วย ?? fallback

2. admin/index.html
   Dashboard - ปัญหาเดียวกัน แก้แบบเดียวกัน

3. request.html
   เพิ่มเช็คเลขบัตร/Passport ซ้ำภายในคำขอเดียวกัน
   ถ้าซ้ำ -> alert + ขอบแดง + เลื่อนไปหา + ไม่ให้ส่ง

4. exam.html
   ยามกันใบเปล่า - ถ้าไม่มีชื่อ จะไม่ยอมออกใบวุฒิบัตร
   ขึ้นข้อความให้ติดต่อ จป. แทน (ดัก 3 จุด)


ยังต้องแก้ที่ Power Automate (ไม่ได้อยู่ในไฟล์นี้)
--------------------------------------------------
Flow 1 CheckTraining_PO7 -> Response ของ case valid และ expired
เปลี่ยนบรรทัด fullName เป็น:

"fullName": "@{if(empty(outputs('TrainingRow')?['FullName']), first(outputs('List_Trainees')?['body/value'])?['FullName'], outputs('TrainingRow')?['FullName'])}"

เพื่อให้คนที่เคยสอบแล้ว (ซึ่ง TrainingData ชื่อว่าง) ถอยไปเอาชื่อจากตาราง Trainees แทน


เพิ่มเติมรอบล่าสุด
------------------
5. exam.html — ฝังลายเซ็น จป. PO7 แล้ว (base64)
   + แก้โค้ดวาดลายเซ็นให้รักษาสัดส่วนเดิม ไม่โดนยืดจนเพี้ยน
   PO5 / PO6 / PO8 ยังเป็น signature: '' รอลายเซ็นจริง
