### การบันทึกผลการทดลอง 

**Table 2.1: Memory Address Analysis**

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|----------------------|-------------|
| Stack | stack_var | 0x3ffb4550 | SRAM |
| Global SRAM | sram_buffer | 0x3ffb16ac | SRAM |
| Flash | flash_string | 0x3f407d64 | Flash |
| Heap | heap_ptr | 0x3ffb526c | SRAM |

**Table 2.2: Memory Usage Summary**

| Memory Type | Free Size (bytes) | Total Size (bytes) |
|-------------|-------------------|--------------------|
| Internal SRAM | 380124 bytes | 520,192 |
| Flash Memory | 0 bytes | varies |
| DMA Memory | 303088 bytes | varies |

# คำถามวิเคราะห์ (ง่าย)

### 1. Memory Types : SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
    SRAM (Static RAM): ใช้เก็บข้อมูลที่โปรแกรมใช้งานขณะรันไทม์ (runtime) เช่น ตัวแปร Global, Local, และข้อมูลที่จัดสรรบน Heap. SRAM เป็นหน่วยความจำที่มีความเร็วสูงมาก, แต่ข้อมูลจะหายไปเมื่อปิดเครื่อง.

    Flash Memory: ใช้เก็บโค้ดโปรแกรม (firmware), ข้อมูลคงที่ (constants), และไฟล์ระบบต่างๆ. Flash Memory เป็นหน่วยความจำแบบไม่ลบเลือน (non-volatile), ข้อมูลจะยังคงอยู่แม้ไม่มีไฟเลี้ยง
### 2. Address Ranges : ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
    จาก Table 2.1, ตัวแปรแต่ละประเภทจะอยู่ในช่วง Address ที่แตกต่างกัน:

    - Stack (stack_var): อยู่ใน Address Range ของ SRAM, ที่อยู่ 0x3ffbxxxx

    - Global SRAM (sram_buffer): อยู่ใน Address Range ของ SRAM, ที่อยู่ 0x3ffbxxxx

    - Flash (flash_string): อยู่ใน Address Range ของ Flash Memory, ที่อยู่ 0x3f40xxxx

    - Heap (heap_ptr): อยู่ใน Address Range ของ SRAM, ที่อยู่ 0x3ffbxxxx
### 3. Memory Usage : ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?
    Internal SRAM: ESP32 มี Internal SRAM ทั้งหมด 520,192 bytes และถูกใช้ไปแล้ว 140,068 bytes (520,192 - 380,124)

    Flash Memory: ข้อมูลในตารางระบุว่ามีการใช้ไป 0 bytes (Free) แต่ Total Size ของ Flash จะแตกต่างกันไปตามรุ่นของ ESP32 chip โดยปกติจะมีขนาดตั้งแต่ 4MB ขึ้นไป

    DMA Memory: มี Free Size 303,088 bytes. DMA เป็นหน่วยความจำย่อยภายใน SRAM ที่ออกแบบมาสำหรับการถ่ายโอนข้อมูลโดยตรงโดยไม่ต้องใช้ CPU