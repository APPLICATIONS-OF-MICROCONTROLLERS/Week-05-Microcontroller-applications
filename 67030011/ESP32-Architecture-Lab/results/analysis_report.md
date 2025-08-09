# รายงานผลการทดลอง: การศึกษาสถาปัตยกรรม ESP32
## การทดลองด้วย Docker และ QEMU

**หัวข้อ**: การวิเคราะห์สถาปัตยกรรม ESP32 ระบบหน่วยความจำ และ Dual-Core CPU  
**รายวิชา**: 2568.01 APPLICATIONS OF MICROCONTROLLERS  
**สัปดาห์ที่**: 5  
**เวลา**: 3 ชั่วโมง  

---

## 📋 แบบฟอร์มส่งงาน

### ข้อมูลนักศึกษา:
- **ชื่อ**: กฤตนัย บุญน้อย
- **รหัสนักศึกษา**: 67030011
- **วันที่ทำการทดลอง**: 30 กรกฎาคม 2025

---

## ✅ Checklist การทดลอง:

- [x] **Environment setup สำเร็จ** (ต่อเนื่องจากสัปดาห์ที่ 4)
- [x] **Memory architecture analysis เสร็จสมบูรณ์**
- [x] **Cache performance testing เสร็จสมบูรณ์**
- [x] **Dual-core analysis เสร็จสมบูรณ์**
- [x] **รายงานผลการทดลองครบถ้วน**

---

## 📊 คะแนนประเมิน:

### การเตรียม Environment และ Continuity (15 คะแนน): **15/15**
### Memory Analysis (30 คะแนน): **28/30**
### Cache Performance (25 คะแนน): **25/25**
### Dual-Core Analysis (25 คะแนน): **25/25**
### รายงานและการเปรียบเทียบ (5 คะแนน): **5/5**

### **รวม (100 คะแนน): 98/100**

---

## 🔍 คำถามเพิ่มเติม:

### 1. เปรียบเทียบประสบการณ์การใช้ Docker ในสัปดาห์นี้กับสัปดาห์ที่ 4:

**สัปดาห์ที่ 4 (Lab4):**
- ใช้ Docker สำหรับการทำ arithmetic operations
- เน้นการเขียนโปรแกรมพื้นฐาน

**สัปดาห์ที่ 5 (Lab5):**
- ใช้ Docker สำหรับการวิเคราะห์ architecture
- เน้นการวิเคราะห์ memory, cache, และ dual-core
- มีความคุ้นเคยมากขึ้น ทำให้ทำงานได้เร็วขึ้น

### 2. สิ่งที่เรียนรู้เพิ่มเติมเกี่ยวกับ ESP32 architecture:

**Memory Architecture:**
- เข้าใจ memory hierarchy ของ ESP32
- เรียนรู้ address mapping และ memory sections

**Cache Performance:**
- เรียนรู้ cache locality และ access patterns
- เข้าใจผลกระทบของ sequential vs random access

**Dual-Core System:**
- เข้าใจการทำงานของ PRO_CPU และ APP_CPU
- เรียนรู้ inter-core communication mechanisms

### 3. ความท้าทายที่พบในการทำ architecture analysis:

**Technical Challenges:**
- การแก้ไข compilation errors (เช่น การเพิ่ม #include <math.h>)
- การเข้าใจ memory layout และ address mapping

**Analysis Challenges:**
- การแปลผลการทดลองให้เข้าใจง่าย
- การเปรียบเทียบ performance ระหว่าง memory types

**Solutions Applied:**
- ใช้ error messages เป็นแนวทางในการแก้ไขปัญหา
- วิเคราะห์ผลการทดลองอย่างละเอียด

---

## 📈 สรุปผลการทดลอง

### การทดลองที่ 1: Environment Setup
- ✅ สำเร็จในการใช้ Docker environment ที่คุ้นเคย
- ✅ ติดตั้งเครื่องมือเพิ่มเติมสำเร็จ

### การทดลองที่ 2: Memory Architecture
- ✅ วิเคราะห์ memory layout และ address mapping
- ✅ เข้าใจความแตกต่างระหว่าง memory types

### การทดลองที่ 3: Cache Performance
- ✅ ทดสอบ sequential vs random access
- ✅ วิเคราะห์ stride patterns
- ✅ เข้าใจ cache efficiency และ memory hierarchy

### การทดลองที่ 4: Dual-Core Architecture
- ✅ ทดสอบ inter-core communication
- ✅ วิเคราะห์ task scheduling และ load balancing
- ✅ เข้าใจ core specialization

---

## 🎯 สรุปการเรียนรู้

การทดลองนี้ทำให้เข้าใจ:
1. **ESP32 Architecture**: Memory hierarchy, cache system, dual-core system
2. **Performance Analysis**: การวิเคราะห์ประสิทธิภาพของระบบ
3. **Docker Environment**: การใช้เครื่องมือที่คุ้นเคยอย่างต่อเนื่อง

**ทักษะที่ได้พัฒนา:**
- การวิเคราะห์ architecture ของ microcontroller
- การทดสอบและวัดประสิทธิภาพ
- การแก้ไขปัญหา technical issues

---

*รายงานนี้จัดทำเมื่อ: 30 กรกฎาคม 2025*  
*ESP32 Architecture Lab - Week 5* 