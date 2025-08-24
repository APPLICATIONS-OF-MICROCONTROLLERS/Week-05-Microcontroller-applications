# ใบงานทดลอง: การศึกษาสถาปัตยกรรม ESP32
## 🔬 การทดลองที่ 1: การเตรียม Environment (ใช้เครื่องมือที่คุ้นเคย)
### คำถามทบทวน

1. **Docker Commands**: คำสั่ง `docker-compose up -d` และ `docker-compose exec esp32-dev bash` ทำอะไร?
   
   ตอบ :
   - `docker-compose up -d` คำสั่งนี้จะทำการสร้างและเริ่ม container ตามที่กำหนดไว้ในไฟล์ docker-compose.yml โดยที่:
   
       `up`: เป็นคำสั่งที่ใช้ในการสร้าง (create) และเริ่มต้น (start) services ใน Docker Compose

       `-d` (detached mode): เป็น flag ที่บอกให้ Docker รัน container ใน background โดยไม่ผูกติดกับ terminal ที่เราใช้งานอยู่ ทำให้เรายังสามารถใช้ terminal นั้นได้ต่อไปในขณะที่ container ทำงานอยู่

   - `docker-compose exec esp32-dev bash` ใช้สำหรับ เข้าถึง และ รันคำสั่ง ภายใน container ที่กำลังทำงานอยู่เพื่อให้เราสามารถจัดการหรือแก้ไขไฟล์ต่างๆ ได้ตามต้องการ


3. **ESP-IDF Tools**: เครื่องมือไหนจาก Lab4 ที่จะใช้ในการ build โปรแกรม ESP32?
   
   ตอบ : idf.pu build
   
4. **New Tools**: เครื่องมือใหม่ที่ติดตั้ง (tree, htop) ใช้ทำอะไร?
   
   ตอบ :
   - tree : ใช้สำหรับแสดงโครงสร้างไดเรกทอรีและไฟล์ต่างๆ ในรูปแบบที่เหมือนต้นไม้
   - htop เป็นเครื่องมือ interactive process viewer หรือโปรแกรมตรวจสอบระบบแบบโต้ตอบ ซึ่งใช้สำหรับมอนิเตอร์สถานะการทำงานของระบบแบบเรียลไทม์
 
5. **Architecture Focus**: การศึกษา ESP32 architecture แตกต่างจากการทำ arithmetic ใน Lab4 อย่างไร?

   ตอบ :
   - โดย ESP32 architecture จะศึกษาโครงสร้างภายในของไมโครคอนโทรลเลอร์ (ระบบ dual-core, memory mapping, cache system)
   - ส่วน arithmetic Lab4  คือการดำเนินการทางคณิตศาสตร์ (บวก ลบ คูณ หาร)

### ผลลัพธ์ที่คาดหวัง
- [ ] สร้างโฟลเดอร์ ESP32-Architecture-Lab เรียบร้อย
- [ ] คัดลอกหรือสร้าง docker-compose.yml ได้สำเร็จ
- [ ] รัน Docker container ได้ปกติ (เหมือน Lab4)
- [ ] เข้า container และใช้ ESP-IDF ได้ (คุ้นเคยจาก Lab4)
- [ ] ติดตั้งเครื่องมือเพิ่มเติม (tree, htop) สำเร็จ
- [ ] สร้าง directories สำหรับการทดลอง architecture เรียบร้อย

### 🤖 ข้อดีของการใช้เครื่องมือที่คุ้นเคย

**เหตุผลที่ใช้ docker-compose.yml เหมือน Lab4:**
- **ไม่ต้องเรียนรู้ใหม่**: ใช้คำสั่งเดิมที่รู้จักแล้ว
- **ประหยัดเวลา**: ไม่เสียเวลาอธิบาย Docker setup ใหม่
- **โฟกัส ESP32**: เน้นที่เนื้อหา architecture ไม่ใช่เครื่องมือ
- **ความมั่นใจ**: ใช้สิ่งที่ work แล้วจาก Lab4
---

## 🔬 การทดลองที่ 2: การศึกษา Memory Architecture
### การบันทึกผลการทดลอง 

**Table 2.1: Memory Address Analysis**

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|----------------------|-------------|
| Stack | stack_var | 0x3ffb4550 | SRAM |
| Global SRAM | sram_buffer | 0x3ffb16ac | SRAM |
| Flash | flash_string | 0x3f407b48 | Flash |
| Heap | heap_ptr | 0x3ffb5264 | SRAM |

**Table 2.2: Memory Usage Summary**

| Memory Type | Free Size (bytes) | Total Size (bytes) |
|-------------|-------------------|--------------------|
| Internal SRAM | 380096 | 520,192 |
| Flash Memory | 2097152 | varies |
| DMA Memory | 303096 | varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
   ตอบ :
   - SRAM : ใช้สำหรับเก็บข้อมูลที่เปลี่ยนแปลงบ่อยและต้องเข้าถึงได้อย่างรวดเร็ว เช่น ตัวแปร  **ข้อมูลจะหายไปเมื่อปิดไฟ
   - Flash Memory: ใช้สำหรับเก็บข้อมูลถาวร เช่น โค้ดโปรแกรม (firmware) ที่เราเขียนและคอมไพล์แล้ว, ข้อมูลที่ตั้งค่าไว้ (configuration data), และ ไฟล์ระบบ ** ข้อมูลยังคงอยู่แม้ไม่มีไฟเลี้ยง
3. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
   ตอบ :
   บน ESP32, ตัวแปรจะถูกจัดเก็บในหน่วยความจำประเภท SRAM และถูกแบ่งออกเป็นหลายส่วน:
   - Global/Static Variables: จัดเก็บในส่วนของ .data หรือ .bss ของ SRAM
   - Local Variables: จัดเก็บในส่วนของ Stack
   - Dynamically Allocated Variables: จัดเก็บในส่วนของ Heap
5. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?
   ตอบ :
   -SRAM : ทั้งหมดประมาณ 520 KB (Kilobytes) ซึ่งใช้เป็นหน่วยความจำหลักสำหรับโปรแกรมที่กำลังรันอยู่
   -Flash : โดยทั่วไปมีตั้งแต่ 4 MB (Megabytes) ขึ้นไป Flash ใช้เก็บโค้ดโปรแกรมและข้อมูลถาวร

---
## 🔬 การทดลองที่ 3: การศึกษา Cache Performance
### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 7299 | 1.16x |
| Random | Internal SRAM | 8489 | 1.16x |
| Sequential | External Memory | 28123 | 1.05x |
| Random | External Memory | 29647 | 1.05x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 7772 us | 1.00x |
| 2 | 4506 us | 0.58x |
| 4 | 1632 us | 0.21x |
| 8 | 809 us | 0.10x |
| 16 | 408 us | 0.05x |

<img width="1206" height="863" alt="image" src="https://github.com/user-attachments/assets/b1c74ba0-6454-463f-bbff-4f5a5e2b2b4e" />

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
   ตอบ :
   - เพราะ เมื่อ CPU เข้าถึงข้อมูลในตำแหน่งหนึ่ง (sequential access) ตัว cache จะคาดการณ์ว่าข้อมูลที่อยู่ติดกันจะถูกเรียกใช้ในลำดับถัดไป จึงดึงข้อมูลชุดใหญ่มาเก็บไว้ใน cache ล่วงหน้า ซึ่งเร็วกว่าการไปดึงข้อมูลจากหน่วยความจำหลัก (RAM) โดยตรงทุกครั้ง ในทางกลับกัน random access จะกระโดดไปมาในหน่วยความจำ ทำให้ cache ไม่สามารถคาดเดาข้อมูลได้ล่วงหน้า จึงต้องดึงข้อมูลใหม่จาก RAM อยู่เสมอ ทำให้ประสิทธิภาพต่ำกว่า 
3. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
   ตอบ :
   - Internal SRAM: เป็นหน่วยความจำขนาดเล็กที่อยู่ในชิป ESP32 โดยตรง มีความเร็วในการเข้าถึงสูงมาก (เกือบเท่ากับความเร็วของ CPU) เหมาะสำหรับเก็บข้อมูลที่จำเป็นต้องเข้าถึงบ่อยๆ เช่น stack และ heap
   - External Memory: เป็นหน่วยความจำภายนอกที่เชื่อมต่อกับ ESP32 (เช่น external flash หรือ PSRAM) มีขนาดใหญ่กว่ามากแต่มีความเร็วในการเข้าถึงที่ช้ากว่าอย่างมีนัยสำคัญ เหมาะสำหรับเก็บโค้ดโปรแกรมขนาดใหญ่หรือข้อมูลจำนวนมากที่ไม่จำเป็นต้องเข้าถึงแบบเรียลไทม์
5. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?
   ตอบ :
   - stride size มีขนาดเล็กและข้อมูลที่เข้าถึงอยู่ติดกันหรือใกล้เคียงกัน จะช่วยให้ CPU ใช้ประโยชน์จาก cache ได้เต็มที่ ทำให้ performance ดีขึ้น เพราะข้อมูลที่ต้องการส่วนใหญ่อยู่ใน cache แล้ว
   - stride size มีขนาดใหญ่มาก ทำให้การเข้าถึงข้อมูลกระโดดข้าม cache line ไปไกลๆ จะทำให้เกิด cache miss บ่อยครั้ง CPU จะต้องไปดึงข้อมูลจาก RAM อยู่ตลอดเวลา ส่งผลให้ performance ลดลง 
---

## 🔬 การทดลองที่ 4: การศึกษา Dual-Core Architecture
### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 74 | 9691 |
| Total Execution Time (ms) | 5002 ms | 5944 ms |
| Task Completion Rate | 100% | 100% |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 10 |
| Messages Received | 10 |
| Average Latency (μs) | 16108 μs |
| Queue Overflow Count | 0 |

<img width="1016" height="950" alt="image" src="https://github.com/user-attachments/assets/3debde19-2e03-4aff-86b8-b74067cac876" />
<img width="723" height="679" alt="image" src="https://github.com/user-attachments/assets/d5870b19-4ca1-4317-83cf-d9817097d1d9" />
<img width="651" height="401" alt="image" src="https://github.com/user-attachments/assets/54671f14-60fb-44d1-993a-cc2a6fcc654c" />

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?

   ตอบ :
   - core 0 เหมาะสำหรับงานที่ต้องจัดการกับ การสื่อสารแบบเรียลไทม์ เช่น Wi-Fi และ Bluetooth ซึ่งเป็นโปรโตคอลหลักของ ESP32 งานประเภทนี้ต้องการความเสถียรและเวลาที่แม่นยำเพื่อป้องกันการขาดการเชื่อมต่อ
   - core 1 เหมาะสำหรับงาน ประมวลผลหลัก หรือ แอปพลิเคชัน (Application) ที่เราเขียนขึ้น เช่น การประมวลผลข้อมูลเซ็นเซอร์, การคำนวณทางคณิตศาสตร์, หรือการจัดการกับ user interface 
3. **Communication Overhead**: inter-core communication มี overhead เท่าไร?

   ตอบ : ประมาณ 12 ms
5. **Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?

   ตอบ : การกระจายงานระหว่าง coresมีประสิทธิภาพสูง  เพราะมีการกระจายงานในทั้ง 2 core

---

