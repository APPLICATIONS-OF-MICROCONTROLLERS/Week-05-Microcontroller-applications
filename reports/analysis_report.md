### แบบฟอร์มส่งงาน

**ข้อมูลนักศึกษา:**
- ชื่อ: นางสาวนิรภาดา ขัตติยะ
- รหัสนักศึกษา: 67030118
- วันที่ทำการทดลอง: 8/02/2025

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
 ในสัปดาห์ที่ 4 เรียนรู้แบบใช้ 1 core แต่สัปดาห์ที่ 5 ใช้ Dual-Core ทำให้ทำงานได้เร็วขึ้นและสามารถแบ่งงานไปแต่ละ Core ได้
2. สิ่งที่เรียนรู้เพิ่มเติมเกี่ยวกับ ESP32 architecture:
   การทำงานของ Core,Memory,หน่วยความจำ,Bus,Peripheral

3. ความท้าทายที่พบในการทำ architecture analysis:
   โค้ดมีการเรียกใช้ฟังก์ชันทางคณิตศาสตร์ แต่ไม่ได้ #include <math.h> ส่งผลให้เกิด error ในขั้นตอนการคอมไพล์ จำเป็นต้องตรวจสอบ header file ให้ครบถ้วนก่อนรัน
   การดึงข้อมูลจากฮาร์ดแวร์ต้องใช้ขั้นตอนและการตั้งค่าหลายส่วน ทำให้การอ่านค่ามีความยุ่งยากและต้องทำความเข้าใจโครงสร้างของฮาร์ดแวร์ก่อน

---

### คำถามทบทวน

1. **Docker Commands**: คำสั่ง `docker-compose up -d` และ `docker-compose exec esp32-dev bash` ทำอะไร?
docker-compose up -d  สร้างและรัน container ตามไฟล์ docker-compose.yml แบบ background
docker-compose exec esp32-dev bash  เข้าไปใช้ shell (bash) ใน container ชื่อ esp32-dev
2. **ESP-IDF Tools**: เครื่องมือไหนจาก Lab4 ที่จะใช้ในการ build โปรแกรม ESP32?
ใช้ idf.py build ในการ build โปรแกรมสำหรับ ESP32
ใช้ idf.py flash ในการอัปโหลดโปรแกรมไปยังบอร์ด
ใช้ idf.py monitor ในการดู log หรือ output จากบอร์ด
3. **New Tools**: เครื่องมือใหม่ที่ติดตั้ง (tree, htop) ใช้ทำอะไร?
tree  แสดงโครงสร้างโฟลเดอร์และไฟล์แบบเป็นชั้น ๆ
htop  ดูการใช้ CPU, RAM และ process ต่าง ๆ แบบ real-time   
4. **Architecture Focus**: การศึกษา ESP32 architecture แตกต่างจากการทำ arithmetic ใน Lab4 อย่างไร?
Architecture  ศึกษาโครงสร้างภายใน ESP32 เช่น CPU, memory hierarchy, peripherals
Arithmetic (Lab4)  เน้นการเขียนโปรแกรมคำนวณเชิงตัวเลข เช่น บวก ลบ คูณ หาร และการประมวลผลข้อมูล
### การบันทึกผลการทดลอง 

**Table 2.1: Memory Address Analysis**

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|----------------------|-------------|
| Stack | stack_var |  0x3ffb4550 | SRAM |
| Global SRAM | sram_buffer | 0x3ffb16ac | SRAM |
| Flash | flash_string | 0x3f407b64 | Flash |
| Heap | heap_ptr | 0x3ffb5264 | SRAM |

**Table 2.2: Memory Usage Summary**

| Memory Type | Free Size (bytes) | Total Size (bytes) |
|-------------|-------------------|--------------------|
| Internal SRAM | 380096 | 520,192 |
| Flash Memory |2 MB| varies |
| DMA Memory | 303096 | varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
SRAM  เก็บข้อมูลที่เปลี่ยนแปลงได้ระหว่างรันโปรแกรม เช่น ตัวแปรใน stack, heap, และ global variables
Flash Memory  เก็บโปรแกรม (firmware) และข้อมูลถาวร เช่น constant string, lookup table, code
2. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
Global / Static Variables ตัวแปร global หรือ static (ทั้ง global static และ local static) Address Range: 0x3FFB_0000 ~ 0x3FFC_0000
Stack Variables ตัวแปรภายในฟังก์ชันที่ ไม่เป็น static (local variables) Address Range: 0x3FFB_0000 ~ 0x3FFC_0000 (ใช้พื้นที่เดียวกับ global แต่จัดการต่างกัน)
Heap Allocated Variables ตัวแปรที่สร้างด้วย malloc(), calloc(), pvPortMalloc() Address Range: Internal SRAM: 0x3FFB_0000 ~ 0x3FFE_FFFF
ใช้ PSRAM : 0x3F80_0000 ~
Read-only Data / Const ข้อมูลแบบ const หรือ string literal ("Hello"), ตาราง lookup Address Range: 0x3F40_0000 ~ 0x3F7F_FFFF
Code / Text Section ฟังก์ชันหรือคำสั่ง (compiled machine code) Address Range: 0x4008_0000 ~ 0x400B_FFFF 
4. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?
มีทั้งหมด 520,192
ใช้ไป = 520,192 − 380,096 = 140,096 bytes
---
### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 	8891 | 1.00x |
| Random | Internal SRAM | 11679 | 1.31x |
| Sequential | External Memory | 11679 | 1.00x |
| Random | External Memory | 35829 | 1.10x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 10411 | 1.00x |
| 2 | 4586 | 0.44x |
| 4 | 2373 | 0.23x |
| 8 | 1229 | 0.12x |
| 16 | 843 | 0.08x |

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
  Sequential access เร็วกว่า เพราะ CPU สามารถดึงข้อมูลล่วงหน้า (prefetch) และใช้ cache ได้เต็มที่
  Random access ช้ากว่า เพราะตำแหน่งกระโดด ทำให้ cache ทำงานไม่ทัน เกิด cache miss บ่อย
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
  Internal SRAM: อยู่ในชิป = เร็ว, latency ต่ำ, bandwidth สูง
  External Memory (PSRAM): อยู่นอกชิป = ช้าเพราะต้องส่งผ่าน bus ภายนอก
4. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?
  Stride เล็ก (1, 2)  อ่านข้อมูลต่อเนื่องใน cache = เร็ว
  Stride ใหญ่ (8, 16)  กระโดดข้าม cache line  cache miss มากขึ้น = ช้า 

---


### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 1234 | 978 |
| Total Execution Time (ms) | 5050 | 4900 |
| Task Completion Rate | 19.80 it/s  | 30.60 it/s |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 10 |
| Messages Received | 10 |
| Average Latency (μs) | 456 |
| Queue Overflow Count | 0 |

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?
  Core 0 (PRO_CPU) → เหมาะกับงาน I/O, Protocol handling, Network stack เพราะเป็น core หลักที่จัดการ interrupt และระบบภายใน
  Core 1 (APP_CPU) → เหมาะกับงาน CPU-intensive / computation เพราะไม่ต้องรับภาระ interrupt มาก
3. **Communication Overhead**: inter-core communication มี overhead เท่าไร?
   มี overhead ประมาณ หลายร้อยถึงหลักพัน µs (ขึ้นกับความยาวข้อความและความถี่ส่ง)
5. **Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?
  ถ้างานแบ่งชัดเจน Core0 = I/O / Core1 = Compute ประสิทธิภาพดี
  ถ้า workload ไม่สมดุล (เช่น Core1 ทำงานหนักกว่า)  เกิด bottleneck และ idle time ในอีก core
  การใช้ monitor metric ช่วยปรับ workload ให้สมดุลจะได้ performance สูงสุด


