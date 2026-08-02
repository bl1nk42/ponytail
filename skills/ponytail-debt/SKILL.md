---  
name: ponytail-debt  
description: >  
  Harvest every `ponytail:` comment in the codebase into a debt ledger, so the  
  deliberate shortcuts and deferrals ponytail leaves behind get tracked instead  
  of rotting into "later means never". Use when the user says "ponytail debt",  
  "/ponytail-debt", "what did ponytail defer", "list the shortcuts", "ponytail  
  ledger", or "what did we mark to do later". One-shot report, changes nothing.  
---  
ทุกการตัดทางเลือกแบบมีเจตนาของ `ponytail` จะถูกระบุไว้ด้วยคอมเมนต์ `ponytail:` ซึ่งระบุขีดจำกัด (ceiling) และเส้นทางอัปเกรด (upgrade path) ไว้ ทำให้สามารถรวบรวมทั้งหมดนี้เข้าเป็นบัญชีหนี้เดียว เพื่อป้องกันไม่ให้การเลื่อนการดำเนินการกลายเป็นถาวรโดยไม่รู้ตัว  

## ตรวจหา  
สแกนโค้ดใน repo หาเครื่องหมายคอมเมนต์ โดยข้ามโฟลเดอร์ `node_modules`, `.git` และผลลัพธ์จากการสร้าง (build output):  
`grep -rnE '(#|//) ?ponytail:' .`  
(เพิ่มสัญลักษณ์คอมเมนต์อื่นๆ หากสแต็กของคุณใช้)

แต่ละจุดที่พบ คือแถวหนึ่งในบัญชีหนี้ ตัวอย่างเช่น `// ponytail: v2.0, upgrade when auth service stabilizes`  
สัญลักษณ์คอมเมนต์ช่วยแยกความแตกต่างระหว่างข้อความที่แค่กล่าวถึงแนวทางกับข้อความที่ตั้งใจจะติดตาม

## ผลลัพธ์  
แสดงผลหนึ่งแถวต่อเครื่องหมาย จัดกลุ่มตามไฟล์:  
`:, . ceiling: . upgrade: .`

รูปแบบที่ใช้คือ `ponytail: , ` ดังนั้นให้ดึงขีดจำกัดและเหตุการณ์กระตุ้นตรงจากคอมเมนต์ได้เลย  
ต้องการระบุผู้รับผิดชอบแต่ละแถวด้วยไหม? ให้เพิ่มคำสั่ง `git blame -L,`

แจ้งความเสี่ยงของการเสื่อมสภาพ: คอมเมนต์ `ponytail:` ที่ไม่ระบุเส้นทางอัปเกรดหรือเหตุการณ์กระตุ้น จะได้รับแท็ก `no-trigger` — นี่คือจุดที่กำลังเสื่อมสภาพโดยเงียบ ๆ

จบด้วย ` markers,  with no trigger.`  
หากไม่พบอะไรเลย: `No ponytail: debt. Clean ledger.`

## ขอบเขตการทำงาน  
อ่านและรายงานเท่านั้น ไม่เปลี่ยนแปลงโค้ดใด ๆ  
หากต้องการเก็บบันทึกไว้ ให้ขอมา ระบบจะเขียนบัญชีหนี้ลงไฟล์ (เช่น `PONYTAIL-DEBT.md`)  
ทำงานเพียงครั้งเดียว ใช้คำสั่ง "stop ponytail-debt" หรือ "normal mode" เพื่อกลับสู่โหมดปกติ