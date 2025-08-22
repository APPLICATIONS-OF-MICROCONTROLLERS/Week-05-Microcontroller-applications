# ใบงานทดลอง: การศึกษาสถาปัตยกรรม ESP32
## คำถามทบทวน
    1.Docker Commands: คำสั่ง docker-compose up -d และ docker-compose exec esp32-dev bash ทำอะไร?
        `docker-compose up -d`
         สั่งให้ Docker Compose สร้างและรัน container ทั้งหมดที่นิยามไว้ในไฟล์ docker-compose.yml แบบ detached mode 
        `docker-compose exec esp32-dev bash`
        สั่งเข้าไปทำงานใน container ที่ชื่อ esp32-dev โดยเปิด shell bash ข้างในนั้น เหมือนเรา “เข้าไปนั่งทำงานในเครื่อง ESP32 dev environment”

    2.ESP-IDF Tools: เครื่องมือไหนจาก Lab4 ที่จะใช้ในการ build โปรแกรม ESP32?
        เครื่องมือหลักที่ใช้ build โปรแกรม ESP32 ได้แก่:
            `idf.py build` → คอมไพล์โค้ดและสร้างไฟล์ binary
            `idf.py flash` → อัปโหลดโปรแกรมไปยังบอร์ด ESP32
            `idf.py monitor` → เปิด serial monitor ดูการทำงานของบอร์ด
            Compiler (เช่น `xtensa-esp32-elf-gcc`) → ตัวแปลโค้ด C ไปเป็น binary

    3.New Tools: เครื่องมือใหม่ที่ติดตั้ง (tree, htop) ใช้ทำอะไร?
        tree
            แสดงโครงสร้างไฟล์และโฟลเดอร์เป็นรูปต้นไม้ มองเห็นโครงสร้างโปรเจกต์ได้ง่าย
        htop
            โปรแกรม monitor ระบบ ดูว่า CPU, RAM, process อะไรใช้งานอยู่ (แบบ realtime)
    4.Architecture Focus: การศึกษา ESP32 architecture แตกต่างจากการทำ arithmetic ใน Lab4 อย่างไร?
        Lab4 (Arithmetic) → เน้นการคำนวณและเขียนโปรแกรมพื้นฐาน เช่น บวก ลบ คูณ หาร (focus อยู่ที่ logic และการคำนวณ)
        Lab5 (Architecture) → เน้นการศึกษา โครงสร้างภายในของ ESP32 เช่น CPU cores, memory, task scheduling, peripherals, การสื่อสารระหว่างส่วนต่างๆ ของสถาปัตยกรรม ซึ่งลึกกว่าการทำ arithmetic เพราะเกี่ยวข้องกับ การจัดการทรัพยากรของระบบจริง

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
| DMA Memory | 303096 bytes | varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
2. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
3. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?

---

