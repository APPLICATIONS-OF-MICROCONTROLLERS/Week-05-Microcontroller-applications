### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 6130 μs | 1.00x |
| Random | Internal SRAM | 7393 μs | 1.21x |
| Sequential | External Memory | 25075 μs | 1.14x |
| Random | External Memory | 28710 μs | 4.09x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 5811 μs | 1.00x |
| 2 | 3320 μs | 0.57x |
| 4 | 1512 μs | 0.26x |
| 8 | 816 μs | 0.14x |
| 16 | 415 μs | 0.07x |

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?