### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 63 μs | 9833 μs |
| Total Execution Time (ms) | 5,000 ms | 4,500 ms |
| Task Completion Rate | 100% | 100% |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 10 |
| Messages Received | 10 |
| Average Latency (μs) | 9833 μs |
| Queue Overflow Count | 0 |

# คำถามวิเคราะห์

### 1. Core Specialization: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?
    Core 0 (PRO_CPU) เหมาะสำหรับงานที่ต้องการความเร็วสูงและต้องทำซ้ำหลายครั้ง เนื่องจากมี Average Time per Iteration ที่ต่ำมาก (63 μs)
### 2. Communication Overhead : inter-core communication มี overhead เท่าไร?
    Overhead ของ inter-core communication เท่ากับ 9,833 μs ซึ่งคือค่า Average Latency ที่ใช้ในการส่งข้อมูลระหว่าง cores
### 3. Load Balancing : การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?
    การกระจายงาน ไม่มีประสิทธิภาพ เนื่องจาก Core 0 (100 Iterations, 5,000 ms) ทำงานได้เร็วกว่า Core 1 (150 Iterations, 4,500 ms)