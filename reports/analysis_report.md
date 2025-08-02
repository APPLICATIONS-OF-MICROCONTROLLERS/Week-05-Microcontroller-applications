**ข้อมูลนักศึกษา:**
- ชื่อ: นายอิทธิกร ทองสิมา 
- รหัสนักศึกษา: 67030261
- วันที่ทำการทดลอง: 2 ส.ค. 2025

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

   ในสัปดาห์ที่ 4 เรียนรู้เเบบใช้เเค่ 1 core เเต่ในสัปดาห์ที่ 5 มีการเรียนรู้การใช้แบบ Dual-Core ในการทำงาน ทำให้สามารถทำงานได้เร็วขึ้นเเบ่งงานไปยังเเต่ละ core ได้

2. สิ่งที่เรียนรู้เพิ่มเติมเกี่ยวกับ ESP32 architecture:

  โครงสร้างฮาร์ดแวร์, การทำงานของ Core, หน่วยความจำ, Bus, Peripheral

3. ความท้าทายที่พบในการทำ architecture analysis:

   code error ไม่ import math.h เเต่มีการเรียกใช้ทำให้ error
   การอ่านค่าของ hard ware ค่อนค่างยาก เเละโค้ดค่อนข่างเข้าใจยาก
---

### คำถามทบทวน

1. **Docker Commands**: คำสั่ง `docker-compose up -d` และ `docker-compose exec esp32-dev bash` ทำอะไร?

docker-compose up -d สร้าง container จาก image รันอยู่เบื้องหลัง ไม่แสดง log บนหน้าจอ
docker-compose exec esp32-dev bash 	เปิด bash shell เข้าไปทำงานใน container esp32-dev
2. **ESP-IDF Tools**: เครื่องมือไหนจาก Lab4 ที่จะใช้ในการ build โปรแกรม ESP32?

idf.py build 
3. **New Tools**: เครื่องมือใหม่ที่ติดตั้ง (tree, htop) ใช้ทำอะไร?

tree	แสดงโครงสร้างไดเรกทอรีในรูปแบบต้นไม้
htop	เครื่องมือตรวจสอบทรัพยากรระบบแบบโต้ตอบ (interactive process viewer)
4. **Architecture Focus**: การศึกษา ESP32 architecture แตกต่างจากการทำ arithmetic ใน Lab4 อย่างไร?

ESP32 architecture มุ่งเน้นที่ โครงสร้างและระบบฮาร์ดแวร์ 
arithmetic มุ่งเน้นไปที่ การคำนวณเชิงตัวเลข และ ตรรกะของสูตร
---

### การบันทึกผลการทดลอง 

**Table 2.1: Memory Address Analysis**

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|----------------------|-------------|
| Stack | stack_var | 0x3ffb4550 | SRAM |
| Global SRAM | sram_buffer | 0x3ffb16ac | SRAM |
| Flash | flash_string | 0x3f407b64 | Flash |
| Heap | heap_ptr | 0x3ffb5264 | SRAM |

**Table 2.2: Memory Usage Summary**

| Memory Type | Free Size (bytes) | Total Size (bytes) |
|-------------|-------------------|--------------------|
| Internal SRAM | 380096 bytes | 520,192 |
| Flash Memory | 2 MB | varies |
| DMA Memory | 303096 bytes| varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?

    SRAM ใช้เก็บข้อมูลชั่วคราว มีความเร็วสูง ข้อมูลจะหายเมื่อปิดเครื่อง 	ใช้เก็บ Stack, Heap, ตัวแปรชั่วคราว
   	Flash Memory ใช้เก็บข้อมูลถาวร ความเร็วต่ำ ใช้เก็บ โปรแกรม, ค่าคงที่, Configuration
2. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?

   - Global / Static Variables ประเภท: ตัวแปร global หรือ static Address Range  0x3FFB_0000 ~ 0x3FFC_0000
   - Stack Variables ประเภท: ตัวแปรภายในฟังก์ชันที่ไม่เป็น static Address Range 0x3FFB_0000 ~ 0x3FFC_0000
   - Heap Allocated Variables ประเภท: ตัวแปรที่ใช้ผ่าน malloc, calloc, pvPortMalloc Address Range 0x3FFB_0000 ~ 0x3FFE_FFFF หรือ 0x3F80_0000 ~ ถ้าใช้ PSRAM
   - Read-only Data / Const ประเภท: ฟังก์ชันหรือคำสั่ง (Instruction) Address Range 0x3F40_0000 ~ 0x3F7F_FFFF
   - Code / Text Section ประเภท: ฟังก์ชันหรือคำสั่ง Address Range 0x4008_0000 ~ 0x400B_FFFF
   
3. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?

    มี memory ทั้งหมด 520192s byte ใช้ไป 380096 bytes

---


### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 5187 | 1.00x |
| Random | Internal SRAM | 7938 | 1.53x |
| Sequential | External Memory | 5057 | 0.97x |
| Random | External Memory | 8195 | 1.62x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 5523 | 1.00x |
| 2 | 2709 | 0.49x |
| 4 | 1487 | 0.27x |
| 8 | 663 | 0.12x |
| 16 | 838 | 0.15x |

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?

    cache ทำงานดีที่สุดกับsequential มันสามารถโหลดข้อมูลหลายๆ ตัวในครั้งเดียวผ่าน cache line ถ้าใช้ random จะต้องโหลดบ่อย ทำให้เกิด miss เยอะ
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?

   - Internal SRAM เร็วกว่าเพราะอยู่ใกล้ CPU และมี cache
   - External memory (PSRAM) ช้ากว่าเพราะต้องผ่าน bus เพิ่ม และอาจไม่มี cache
3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?

   ยิ่ง stride ใหญ่ Cache efficiency จะดีขึ้นแต่ ถ้าใหญ่เกินไป จะข้าม cache line จน cache miss บ่อยขึ้น

---

### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 122 | 9679 |
| Total Execution Time (ms) | 4992 | 5945 |
| Task Completion Rate | 20.04 iter/sec	 | 25.23 iter/sec|

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 100 |
| Messages Received | 10 |
| Average Latency (μs) | 12,029 |
| Queue Overflow Count | 1 |

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?

core 0 (PRO_CPU) เหมาะกับ งานที่ต้องการความเร็วสูง, ทำซ้ำบ่อย, เช่น การเก็บข้อมูล sensor, การส่งข้อความ, หรือการจัดการ logic loop
Core 1 (APP_CPU) เหมาะกับ งานที่มีการประมวลผลนาน หรือ latency-sensitive, เช่น การรับและประมวลผลข้อมูล, การทำงานด้าน network
2. **Communication Overhead**: inter-core communication มี overhead เท่าไร?

12,029 μs
3. **Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?

มีประสิทธิภาพเพราะสามารถเเบ่งงานกันทำได้ดี เเละเเบ่งงานที่ทำงานได้ดีของเเต่ละ core ได้ ทำให้สามารถทำงานได้เร็ว
---

