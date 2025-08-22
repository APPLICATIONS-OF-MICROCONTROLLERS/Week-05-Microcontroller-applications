### แบบฟอร์มส่งงาน

**ข้อมูลนักศึกษา:**
- ชื่อ: น.ส.นัจญมา จันทร์ชู
- รหัสนักศึกษา:67030110

**Checklist การทดลอง:**
- [ ] Environment setup สำเร็จ (ต่อเนื่องจากสัปดาห์ที่ 4)
- [ ] Memory architecture analysis เสร็จสมบูรณ์
- [ ] Cache performance testing เสร็จสมบูรณ์
- [ ] Dual-core analysis เสร็จสมบูรณ์
- [ ] รายงานผลการทดลองครบถ้วน

**คะแนนประเมิน:**
- การเตรียม Environment และ Continuity (15 คะแนน): _______
- Memory Analysis (30 คะแนน): _______
- Cache Performance (25 คะแนน): _______
- Dual-Core Analysis (25 คะแนน): _______
- รายงานและการเปรียบเทียบ (5 คะแนน): _______
- **รวม (100 คะแนน): _______**

**คำถามเพิ่มเติม:**
1. เปรียบเทียบประสบการณ์การใช้ Docker ในสัปดาห์นี้กับสัปดาห์ที่ 4:
   - docker-compose up -d สร้าง container จาก image รันอยู่เบื้องหลัง ไม่แสดง log บนหน้าจอ
   - docker-compose exec esp32-dev bash 	เปิด bash shell เข้าไปทำงานใน container esp32-dev

2. สิ่งที่เรียนรู้เพิ่มเติมเกี่ยวกับ ESP32 architecture:
   - ESP32 มี SRAM + Flash แยกหน้าที่ชัดเจน (SRAM = ตัวแปร/การทำงาน, Flash = เก็บโปรแกรมถาวร)
   - มีหลาย address space เช่น Data RAM, IRAM, Flash mapping, RTC memory
   - สามารถวิเคราะห์ Memory usage ด้วย idf.py size เพื่อดูว่าโปรแกรมใช้พื้นที่ไปเท่าไร

3. ความท้าทายที่พบในการทำ architecture analysis:
   - ปัญหา CMake / idf.py build error (เช่น CMakeLists.txt ไม่ครบ)
   - การเข้าใจ address range ของตัวแปรแต่ละประเภท
   - ต้องแยกแยะว่า memory ที่ใช้จริง (SRAM/Flash) ไม่ตรงกับขนาดทั้งหมดที่มี
   - การตั้งค่า Docker environment ให้ เรียกใช้ idf.py ได้ถูกต้อง
---