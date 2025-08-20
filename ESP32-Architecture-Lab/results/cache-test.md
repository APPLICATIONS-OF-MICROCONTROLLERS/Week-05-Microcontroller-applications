### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | _______ | 1.00x |
| Random | Internal SRAM | _______ | ____x |
| Sequential | External Memory | _______ | ____x |
| Random | External Memory | _______ | ____x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | _______ | 1.00x |
| 2 | _______ | ____x |
| 4 | _______ | ____x |
| 8 | _______ | ____x |
| 16 | _______ | ____x |

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?