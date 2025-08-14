# Memory Address Analysis (ESP32)

## Table 2.1: Memory Address Analysis

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|------------------------|-------------|
| Stack          | `stack_var`       | `0x3FFBEE40`           | SRAM        |
| Global SRAM    | `sram_buffer`     | `0x3FFB8000`           | SRAM        |
| Flash          | `flash_string`    | `0x400D1234`           | Flash       |
| Heap           | `heap_ptr`        | `0x3FFE2A10`           | SRAM        |

---

## Table 2.2: Memory Usage Summary

| Memory Type    | Free Size (bytes) | Total Size (bytes) |
|----------------|-------------------|--------------------|
| Internal SRAM  | `280,000`         | 520,192            |
| Flash Memory   | `1,200,000`       | varies (ขึ้นกับขนาด Flash บนบอร์ด) |
| DMA Memory     | `120,000`         | varies             |

---

## คำตอบคำถามวิเคราะห์

### 1. Memory Types
- **SRAM**: ใช้เก็บข้อมูลที่เปลี่ยนแปลงได้ระหว่างการทำงานของโปรแกรม เช่น ตัวแปรในฟังก์ชัน (stack), ตัวแปร global, ข้อมูลที่จองด้วย `malloc()` (heap)  
- **Flash Memory**: ใช้เก็บโปรแกรม (code) และค่าคงที่ (constant data เช่น string แบบ `const`) ซึ่งไม่เปลี่ยนระหว่างการทำงาน

### 2. Address Ranges (ของ ESP32 ทั่วไป)
- **Stack / Heap / Global SRAM**: เริ่มที่ `0x3FFBxxxx` หรือ `0x3FFExxxx`  
- **Flash Memory**: เริ่มที่ `0x400Dxxxx` (สำหรับโค้ด) และ `0x3F40xxxx` (สำหรับข้อมูลคงที่)

### 3. Memory Usage
- **Internal SRAM**: มีทั้งหมด **520,192 bytes** (≈ 508 KB) ใช้อยู่ ≈ **240 KB** เหลือ ≈ **280 KB**  
- **Flash Memory**: ขนาดขึ้นกับบอร์ด (เช่น 4MB, 8MB, 16MB) ในตัวอย่างนี้เหลือประมาณ **1.2 MB**  
- **DMA Memory**: เหลือประมาณ **120 KB** (ใช้สำหรับการส่งข้อมูลกับอุปกรณ์ภายนอกเช่น Wi-Fi, SPI, I2S)

## ตาราง 3.1 ผลการทดสอบเวลาในการเข้าถึงหน่วยความจำ (Stride Access Time)

| Stride Size | Time (μs) | Sum           |
|-------------|-----------|---------------|
| 1           | 13174     | (ไม่มีข้อมูล) |
| 2           | 4364      | 1801306112    |
| 4           | 3078      | 3044552704    |
| 8           | 1587      | 3666176000    |
| 16          | 679       | 1829504000    |

---

## ตาราง 3.2 อัตราส่วนเวลาเมื่อเทียบกับ Stride 1

| Stride Size | Ratio vs Stride 1 |
|-------------|-------------------|
| 1           | 1.00x             |
| 2           | 0.33x             |
| 4           | 0.24x             |
| 8           | 0.12x             |
| 16          | 0.05x             |
## วิเคราะห์ผลการทดลอง

จากผลการทดลอง พบว่าเมื่อค่า Stride มีขนาดใหญ่ขึ้น เวลาในการเข้าถึงหน่วยความจำ (Access Time) ลดลงอย่างมีนัยสำคัญ  
ซึ่งสามารถอธิบายได้ดังนี้:

1. **Stride 1** มีเวลาการเข้าถึงมากที่สุด (13174 μs) เนื่องจากต้องเข้าถึงข้อมูลที่อยู่ติดกันในหน่วยความจำ ทำให้เกิดการโหลดบล็อกข้อมูลบ่อยครั้งและมีการใช้ Cache อย่างต่อเนื่อง  
2. **Stride 2 – 16** แสดงให้เห็นว่าค่าเวลาลดลงตามการเพิ่มระยะ Stride เพราะระบบสามารถข้ามตำแหน่งในหน่วยความจำและลดการเข้าถึงข้อมูลที่ไม่จำเป็นใน Cache  
3. อัตราส่วนเวลา (Ratio) ลดลงชัดเจน เช่น Stride 16 ใช้เวลาเพียง **0.05 เท่า** ของ Stride 1 ซึ่งสะท้อนถึงการใช้ Cache อย่างมีประสิทธิภาพมากขึ้น


### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|------------------|------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 150 | 11157 |
| Total Execution Time (ms) | 5000 | 6242 |
| Task Completion Rate | 20 iter/sec | 24 iter/sec |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 90 |
| Messages Received | 90 |
| Average Latency (μs) | ~19212 |
| Queue Overflow Count | 0 |

---

### คำถามวิเคราะห์

1. **Core Specialization**  
   - **Core 0** (PRO_CPU) เหมาะกับงานที่ต้องการความเร็วสูง, รอบการทำซ้ำสั้น ๆ, และ latency ต่ำ (เช่น real-time control)  
   - **Core 1** (APP_CPU) เหมาะกับงานที่ใช้เวลาประมวลผลนานกว่า หรือ background tasks

2. **Communication Overhead**  
   - ค่าเฉลี่ย latency ของการส่งข้อความอยู่ที่ ~19.2 ms ซึ่งค่อนข้างสูงสำหรับงานที่ต้องการสื่อสารบ่อย ๆ  
   - แสดงให้เห็นว่าการส่งข้อมูลระหว่าง core มี overhead พอสมควร

3. **Load Balancing**  
   - Core 1 ใช้เวลาต่อ iteration มากกว่า Core 0 ประมาณ 74 เท่า ทำให้การกระจายงานไม่สมดุล  
   - ถ้าต้องการประสิทธิภาพสูง ควรให้ Core 1 ทำงานที่ใช้ CPU หนักน้อยลง หรือแบ่งงานให้ Core 0 ช่วย


