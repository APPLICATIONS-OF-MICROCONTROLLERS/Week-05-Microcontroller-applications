
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
   ans   1.SRAM: เก็บตัวแปรชั่วคราว เช่น local, global, heap
         2. Flash: เก็บโปรแกรม, ข้อมูลคงที่ (const), ไฟล์
   
3. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
   ans
   |ประเภทข้อมูล	|ตัวอย่าง Address Range |	อธิบาย|
   |------|------|
   |Flash (ROM)|	0x3f40_0000 – 0x3f7f_ffff	|โค้ดโปรแกรม + string คงที่ (const char*) |
   |Data (SRAM, .data/.bss)|	0x3ffb_0000 – 0x3ffd_ffff |	ตัวแปร global/static ที่เปลี่ยนค่าได้ |
   |Heap (malloc/new) |	0x3ffb_xxxx – 0x3ffd_xxxx	 | ขึ้นอยู่กับขนาด heap ที่เหลือ |
   |Stack |	0x3ffb_4xxx, 0x3ffb_5xxx เป็นต้น |	ตัวแปรในฟังก์ชัน (local)|
   |PSRAM (ถ้ามี)	| 0x3f80_0000 ขึ้นไป|	หน่วยความจำเพิ่มพิเศษ (เช่น 4MB) สำหรับเก็บข้อมูลใหญ่ |
5. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?
 -SRAM ทั้งหมด: ~380,096 bytes (≈ 371 KB)
-Heap ว่าง: 303,096 bytes
-Flash: ไม่ระบุขนาด, โดยทั่วไป = 2–4 MB

