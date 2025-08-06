### การบันทึกผลการทดลอง 

**Table 2.1: Memory Address Analysis**

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|----------------------|-------------|
| Stack | stack_var | 0x3ffb4550 | SRAM |
| Global SRAM | sram_buffer | 0x3ffb16ac | SRAM |
| Flash | flash_string | 0x3f407d24 | Flash |
| Heap | heap_ptr | 0x3ffb526c | SRAM |

**Table 2.2: Memory Usage Summary**

| Memory Type | Free Size (bytes) | Total Size (bytes) |
|-------------|-------------------|--------------------|
| Internal SRAM | 380136 | 520,192 |
| Flash Memory | _________ | varies |
| DMA Memory | 303088 | varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
รวดเร็วแต่เก็บแบบจำกัด
2. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
ตัวแปรแบบ global/static จะอยู่ใน DRAM บริเวณ address range ประมาณ 0x3FFB_0000 ขึ้นไป
ตัวแปรแบบ local (stack) จะถูกจัดเก็บใน DRAM เช่นกัน โดย stack ของแต่ละ task จะอยู่ในช่วงที่แยกกัน
ตัวแปรที่ถูกสร้างด้วย malloc/new (heap) จะอยู่ใน DRAM หรือ PSRAM ถ้ามี โดย heap เริ่มที่ 0x3FFB_0000
ตัวแปรแบบ const หรือ string literal จะอยู่ใน Flash memory ในส่วนของ .rodata ซึ่งอยู่ประมาณ 0x3F40_0000 ขึ้นไป
โค้ดโปรแกรม (.text) จะอยู่ใน Flash (เริ่มที่ 0x4008_0000) และบางส่วนที่ต้อง execute เร็วจะถูกโหลดไว้ใน IRAM
3. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?
ESP32 มีหน่วยความจำภายในทั้งหมดประมาณ 520 KB (แบ่งเป็น DRAM และ IRAM) และ Flash memory ที่มักมีตั้งแต่ 4 MB ขึ้นไป (แล้วแต่โมดูล) โดยหน่วยความจำสามารถแบ่งเป็น:
IRAM (เก็บ instruction code ที่ต้อง execute อย่างรวดเร็ว): ประมาณ 128 KB
DRAM (เก็บ data, heap, stack): ประมาณ 328 KB
RTC fast/slow memory สำหรับ deep sleep และ low-power mode
Flash memory (เก็บโปรแกรม, rodata, filesystem): มักเป็น 4MB ขึ้นไป
PSRAM (ถ้ามี): เพิ่มเติมอีก 2MB หรือมากกว่า สำหรับ heap และ buffer ขนาดใหญ่

### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 5076 μs | 1.00x |
| Random | Internal SRAM | 5829 μs	 | 1.15x |
| Sequential | External Memory | 15992 μs	 | 1.00x |
| Random | External Memory | 18314 μs	 | 1.15x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 4805 μs	 | 1.00x |
| 2 | 2296 μs	 | 0.48x |
| 4 | 1231 μs	 | 0.26x |
| 8 | 567 μs   | 0.12x |
| 16 | 307 μs	 | 0.06x |

### คำถามวิเคราะห์
1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
เพราะ sequential access ใช้ประโยชน์จาก spatial locality ทำให้ cache โหลดข้อมูลแบบ block ได้มีประสิทธิภาพ ลดจำนวน cache misses
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
Internal SRAM เร็วกว่าและมี latency ต่ำ ส่วน external memory ช้ากว่าและมี latency สูงกว่า แต่มีขนาดใหญ่และต้นทุนต่ำกว่า
3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?
Stride size ใหญ่เกินไปทำให้ cache miss บ่อยขึ้น ลด performance เพราะข้อมูลที่โหลดมาใน cache ไม่ถูกใช้

### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100	 | 150 |
| Average Time per Iteration (μs) | 51 | 9677 |
| Total Execution Time (ms) | 4992 | 5944 |
| Task Completion Rate | 100%	 | 100% |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 10 |
| Messages Received | 10 |
| Average Latency (μs) | ~14,042 |
| Queue Overflow Count | 0 |

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?
Core0 เหมาะกับงานควบคุมและการจัดการ ส่วน Core1 เหมาะกับงานประมวลผลหนักและงานขนาน
2. **Communication Overhead**: inter-core communication มี overhead เท่าไร?
มี overhead เล็กน้อยจากการซิงโครไนซ์และการส่งข้อมูล แต่โดยรวมยังคุ้มค่าสำหรับงานที่ต้องการ parallelism
3.**Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?
ถ้ากระจายงานอย่างเหมาะสม จะช่วยเพิ่มประสิทธิภาพ ลดเวลาการประมวลผลโดยรวมได้ดี
---
