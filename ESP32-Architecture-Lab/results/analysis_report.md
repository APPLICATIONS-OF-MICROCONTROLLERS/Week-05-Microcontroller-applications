# รายงานสรุปผลการทดลอง ESP32 Architecture Lab

## 1. Memory Architecture Analysis
- **Stack variable address:** 0x3ffb4550 (SRAM)
- **Global SRAM buffer address:** 0x3ffb16ac (SRAM)
- **Flash string address:** 0x3f407b8c (Flash)
- **Heap allocation address:** 0x3ffb5264 (SRAM)
- **Free heap size:** 303096 bytes
- **Internal SRAM:** 380096 bytes
- **DMA capable memory:** 303096 bytes

**สรุป:**
- ตัวแปรแต่ละประเภทถูกจัดเก็บใน memory section ที่เหมาะสม (stack/heap/SRAM/Flash)
- Heap และ SRAM มีขนาดเพียงพอสำหรับงาน embedded

---

## 2. Cache Performance Analysis
### Test 1: Sequential vs Random Access (Internal SRAM)
- Sequential: 6658 μs
- Random: 7279 μs
- Ratio: 1.09x

### Test 2: External Memory Access
- Sequential: 25525 μs
- Random: 27826 μs
- Ratio: 1.09x
- External/Internal Speed Ratio: 3.83x

### Test 3: Stride Access Patterns
- Stride 1: 6892 μs
- Stride 2: 3308 μs
- Stride 4: 2069 μs
- Stride 8: 892 μs
- Stride 16: 461 μs

**สรุป:**
- Sequential access เร็วกว่า random access เล็กน้อย (cache ช่วยลด latency)
- External memory ช้ากว่า internal SRAM 3-4 เท่า
- Stride access ยิ่ง stride ใหญ่ เวลา access ยิ่งน้อยลง (cache line ถูกใช้เต็มประสิทธิภาพ)

---

## 3. Dual-Core Analysis
- **Core 0 total iterations:** 100 (avg: 44 μs)
- **Core 1 total iterations:** 150 (avg: 9660 μs)
- **Messages Sent/Received:** 10/10
- **Average Latency:** ~13,000 μs
- **Task Completion Rate:** 100%

**สรุป:**
- การทำงานแบบ dual-core ช่วยให้สามารถแบ่งงานและสื่อสารระหว่าง core ได้อย่างมีประสิทธิภาพ
- Core 0 (PRO_CPU) เหมาะกับงานที่ต้องการความเร็วสูง/real-time
- Core 1 (APP_CPU) เหมาะกับงานที่ต้องการประมวลผลมากขึ้นหรือ background task
- Inter-core communication latency อยู่ในระดับที่ยอมรับได้สำหรับงาน embedded

