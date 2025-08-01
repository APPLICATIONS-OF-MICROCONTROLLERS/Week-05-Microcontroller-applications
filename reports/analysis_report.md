
 ### การทดลองที่ 1
## คำถามทบทวน

1. Docker Commands: คำสั่ง docker-compose up -d และ docker-compose exec esp32-dev bash ทำอะไร?
   - docker-compose up -d → เริ่ม container
 - docker-compose exec esp32-dev bash → เข้าไปทำงานใน container นั้น เช่น สร้างโปรแกรม ESP32
2. ESP-IDF Tools: เครื่องมือไหนจาก Lab4 ที่จะใช้ในการ build โปรแกรม ESP32?
   - idf.py build
3. New Tools: เครื่องมือใหม่ที่ติดตั้ง (tree, htop) ใช้ทำอะไร?
 
  - `tree`      ดูโครงสร้างโฟลเดอร์แบบต้นไม้   
  - `htop`      ดูการใช้ CPU, RAM แบบ real-time 
  4. Architecture Focus: การศึกษา ESP32 architecture แตกต่างจากการทำ arithmetic ใน Lab4 อย่างไร?
- การศึกษา ESP32 architecture เน้นความเข้าใจ โครงสร้างภายในของชิป เช่น หน่วยประมวลผล, หน่วยความจำ, และอุปกรณ์ต่อพ่วง
 - ส่วน Lab4 (Arithmetic) เน้นการฝึกเขียนโปรแกรมพื้นฐาน (เช่น บวก ลบ คูณ หาร) เพื่อเข้าใจ ภาษา C และตรรกะโปรแกรม
------------------------------------------
### การทดลองที่ 2
![IMG_6751](https://github.com/user-attachments/assets/20248fad-0b7c-47d2-8147-769c7999db1c)
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
| Internal SRAM | 380096 | 520,192 |
| Flash Memory | 303096 | varies |
| DMA Memory | 172032| varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
     - SRAM: เก็บตัวแปรชั่วคราว เช่น local, global, heap
   - Flash: เก็บโปรแกรม, ข้อมูลคงที่ (const), ไฟล์
   
3. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
   
| ประเภทข้อมูล| ประเภทข้อมูล |
|------|------|
| Flash (ROM)   | โค้ดโปรแกรม + string คงที่ (`const char*`)   |
| Data (SRAM, .data/.bss)   | ตัวแปร global/static ที่เปลี่ยนค่าได้   |
| Heap (malloc/new)   | 	ขึ้นอยู่กับขนาด heap ที่เหลือ   |
| 	Stack   | 	ตัวแปรในฟังก์ชัน (local)  |
| PSRAM (ถ้ามี)  | 	หน่วยความจำเพิ่มพิเศษ (เช่น 4MB) สำหรับเก็บข้อมูลใหญ่  |
5. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?
 - SRAM ทั้งหมด: ~380,096 bytes (≈ 371 KB)
- Heap ว่าง: 303,096 bytes
- Flash: ไม่ระบุขนาด, โดยทั่วไป = 2–4 MB
------------------------------------------
### การทดลองที่ 3
<img width="992" height="597" alt="test3" src="https://github.com/user-attachments/assets/f70972d0-f0ed-4e53-8c91-d258d63fa04a" />

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 16460  | 0.97x |
| Random | Internal SRAM | 15952 | 0.97x |
| Sequential | External Memory | 30702 | 1.13x |
| Random | External Memory | 34542 | 1.13x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 14691 | 1.00x |
| 2 | 7096 | 0.48x |
| 4 | 3767 | 0.26x |
| 8 | 3303 | 0.22x |
| 16 | 1289 | 0.09x |

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
   - Sequential access เข้าถึงข้อมูลที่อยู่ติดกันในหน่วยความจำ ทำให้ CPU cache สามารถ โหลดล่วงหน้า (prefetch) ได้หลายค่าภายในหนึ่งครั้ง ซึ่งช่วยลดเวลาในการเข้าถึงข้อมูล
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
   - Internal SRAM: เป็นหน่วยความจำภายในชิป ESP32 → ความเร็วสูง, เข้าถึงได้เร็วมาก
   - External Memory (PSRAM): เป็นหน่วยความจำภายนอกที่เชื่อมต่อผ่าน SPI bus → มี latency สูงกว่า เพราะต้องส่งสัญญาณผ่านบัสภายนอก
3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?
   - Stride size คือจำนวนช่องที่กระโดดข้ามในแต่ละการเข้าถึง array
   - Stride size ที่เล็ก (เช่น 1) ทำให้เข้าถึงข้อมูลต่อเนื่องกัน → ใช้ cache ได้มีประสิทธิภาพ → ทำงานเร็วแต่เมื่อ stride ใหญ่ขึ้น (เช่น 8 หรือ 16) → ข้ามข้อมูลเยอะ → cache ใช้งานได้น้อยลง → ต้องโหลดจากหน่วยความจำบ่อย → ประสิทธิภาพลดลง
------------------------------
### การทดลองที่ 4
### การบันทึกผลการทดลอง
<img width="902" height="572" alt="test4" src="https://github.com/user-attachments/assets/466edde3-bad4-4bb7-8b9c-4050031810f5" />

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 82| 9685 |
| Total Execution Time (ms) | 4998 | 5955 |
| Task Completion Rate | 100% | 100% |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 90 |
| Messages Received | 90 |
| Average Latency (μs) | 28484 |
| Queue Overflow Count | 0 |

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?
   - Core 0 ใช้เวลาต่อ iteration น้อยมาก (เฉลี่ย 63 μs) เหมาะกับ งานที่ต้องการความเร็วและตอบสนองไว เช่น งานควบคุมอุปกรณ์, การจัดการ protocol
   - Core 1 ใช้เวลาต่อ iteration นานกว่ามาก (เฉลี่ย 9694 μs) เพราะมีการคำนวณเชิง floating-point เหมาะกับ งานคำนวณหนัก / background processing เช่น การประมวลผลข้อมูล
2. **Communication Overhead**: inter-core communication มี overhead เท่าไร?
   - การส่งข้อความระหว่าง Core มี latency เฉลี่ยประมาณ 12 ms ต่อข้อความ → ถือว่า มี overhead พอสมควร โดยเฉพาะถ้า Core 1 ยุ่งกับงานคำนวณ
3. **Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?
   - งานถูกกระจายได้ดี ทั้งสอง Core ทำงานเสร็จใกล้เคียงกัน แม้ภาระต่างกัน → แสดงว่า FreeRTOS จัดการ load ได้มีประสิทธิภาพในระดับหนึ่ง

---
## แบบฟอร์มส่งงาน

**ข้อมูลนักศึกษา:**
- ชื่อ: นาย รัฐภูมิ สอนเขียว
- รหัสนักศึกษา: 67030338
- วันที่ทำการทดลอง: 8/1/2025

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
   - การใช้ Docker สัปดาห์นี้มีความต่อเนื่องจากครั้งก่อน ทำให้ setup ง่ายขึ้น ไม่ต้องติดตั้งเครื่องมือซ้ำ ช่วยให้โฟกัสที่โค้ดและการทดลองได้มากขึ้น และมีความยากมากกว่าคร้งก่อน

2. สิ่งที่เรียนรู้เพิ่มเติมเกี่ยวกับ ESP32 architecture:
   - ได้เข้าใจการทำงานของ cache memory, internal SRAM vs external PSRAM และการทำงานของ dual-core (PRO_CPU vs APP_CPU) อย่างมาก รวมถึงการใช้ FreeRTOS ในการกระจายงาน

3. ความท้าทายที่พบในการทำ architecture analysis:
   - code error
   - idf.py qemu monitor ไม่ขึ้น ต้องใช้ idf.py qemu
   - ตีโจทย์ไม่แตก เลยโดนโจทย์ตีแทนครับ


---

