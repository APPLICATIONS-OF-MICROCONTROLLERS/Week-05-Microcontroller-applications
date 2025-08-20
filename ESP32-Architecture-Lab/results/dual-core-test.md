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

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด?
2. **Communication Overhead**: inter-core communication มี overhead เท่าไร?
3. **Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่?