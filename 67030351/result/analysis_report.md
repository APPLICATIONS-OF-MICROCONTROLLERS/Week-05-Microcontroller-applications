# สรุปผลการทดลอง
## 1. Environment Setup
- สร้าง environment ด้วย Docker และ ESP-IDF สำเร็จ
- ใช้ Docker image เดิมจาก Lab 4 ทำให้ setup ง่ายและรวดเร็ว
- ติดตั้งเครื่องมือพื้นฐานสำหรับวิเคราะห์ เช่น tree, htop
### ✅ ผลลัพธ์: ระบบพร้อมสำหรับการทดลองทั้งหมด
## 2. Memory Architecture Analysis
- ทดสอบและวิเคราะห์ตำแหน่งของหน่วยความจำ (stack, heap, flash, SRAM)
- ใช้คำสั่ง xtensa-esp32-elf-objdump และ xtensa-esp32-elf-size เพื่อดู memory sections
- จำลองผลลัพธ์ด้วย QEMU และแสดง memory address อย่างชัดเจน
### ✅ ผลลัพธ์:
- เห็นความแตกต่างระหว่าง stack, global, heap และ flash memory
- เข้าใจ memory layout ของ ESP32
## 3. Cache Performance
- เปรียบเทียบ performance การเข้าถึง memory แบบ sequential, random และ stride
- วิเคราะห์ความแตกต่างระหว่าง internal SRAM และ external PSRAM (หากมี)
- สรุปผลลัพธ์ในรูปแบบตารางเปรียบเทียบเวลา
### ✅ ผลลัพธ์:
- Sequential access เร็วกว่ามากเนื่องจาก cache locality
- Random access และ stride ขนาดใหญ่ทำให้ performance ลดลง
- External PSRAM ช้ากว่า internal SRAM อย่างมีนัยสำคัญ
## 4. Dual-Core Architecture
- สร้าง task 2 ตัวที่ทำงานบน Core0 และ Core1 (PRO_CPU และ APP_CPU)
- ใช้ FreeRTOS queue และ semaphore สื่อสารข้าม core
- ตรวจสอบ performance และ latency
### ✅ ผลลัพธ์:
- Core0 เหมาะสำหรับงานความเร็วสูง (latency ต่ำ)
- Core1 เหมาะสำหรับงานคำนวณหนัก
- มีการสื่อสารระหว่าง cores อย่างราบรื่น
- Queue ไม่ overflow และ latency อยู่ในระดับยอมรับได้
