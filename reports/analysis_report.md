ข้อมูลนักศึกษา:
    • ชื่อ: นวพล สวัสดี
    • รหัสนักศึกษา: 67030109
    • วันที่ทำการทดลอง: 12/08/2568

Checklist การทดลอง:
    [ ✔ ] Environment setup สำเร็จ (ต่อเนื่องจากสัปดาห์ที่ 4)
    [ ✔ ] Memory architecture analysis เสร็จสมบูรณ์
    [ ✔ ] Cache performance testing เสร็จสมบูรณ์
    [ ✔ ] Dual-core analysis เสร็จสมบูรณ์
    [ ✔ ] รายงานผลการทดลองครบถ้วน

คะแนนประเมิน:
    • การเตรียม Environment และ Continuity (15 คะแนน): [ ✔ ]
    • Memory Analysis (30 คะแนน): [ ✔ ]
    • Cache Performance (25 คะแนน): [ ✔ ]
    • Dual-Core Analysis (25 คะแนน): [ ✔ ]
    • รายงานและการเปรียบเทียบ (5 คะแนน): [ ✔ ]
    • รวม (100 คะแนน): [ ✔ ]

คำถามเพิ่มเติม:
เปรียบเทียบประสบการณ์การใช้ Docker ในสัปดาห์นี้กับสัปดาห์ที่ 4: 
    ได้เข้าไปแก้ไข ในไฟล์ docker-compose

สิ่งที่เรียนรู้เพิ่มเติมเกี่ยวกับ ESP32 architecture:
    ความแตกต่างระหว่าง SRAM และ Flash Memory
    การทำงานของ cache และ memory hierarchy ใน ESP32
    Address range ของตัวแปรแต่ละประเภท (stack, heap, global)
    การจัดการ multi-core และ inter-core communication
    วิธีตรวจสอบการใช้ memory จริงผ่าน API และ debugging log

ความท้าทายที่พบในการทำ architecture analysis:
    การเข้าใจ address mapping ของตัวแปรแต่ละชนิดใน memory space
    การแยกแยะระหว่าง memory type (SRAM, Flash, RTC SRAM) และการใช้งานจริง
    การวัดและตีความข้อมูล memory usage จาก log และ debugging tools
    ความซับซ้อนของ multi-core synchronization และ overhead ในการสื่อสาร
    การจัดการข้อมูลที่กระจายอยู่หลายส่วนของ memory และ cache effects
