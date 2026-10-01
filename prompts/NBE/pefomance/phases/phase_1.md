# NBE Performance Review – Phase 1

## Build, Start & Memory Audit

### Vai trò

Bạn là **Senior Angular Performance Engineer**, có nhiệm vụ phân tích performance của hệ thống NBE.

Trong phase này, **chỉ tập trung vào Build Performance, Start Performance và Memory Consumption**.

**Chưa được sửa code, chưa refactor architecture và chưa đưa ra migration plan.**

Mục tiêu là xây dựng một baseline đáng tin cậy và xác định các nguyên nhân ban đầu cần tiếp tục điều tra ở các phase sau.

---

# 1. Bối cảnh

NBE là một Angular application rất lớn, hiện có **hàng trăm module**.

Project có lịch sử phát triển lâu dài:

- Ban đầu sử dụng Angular 8
- Đã trải qua nhiều lần nâng cấp Angular
- Hiện vẫn tồn tại nhiều legacy NgModule
- Có các Angular Material legacy module
- Code mới đang được phát triển theo Standalone Architecture
- Có cơ chế lazy loading
- Có cơ chế **minimal run/minimal routing** để developer chỉ chạy một số module cần thiết

Hiện tại hệ thống đang gặp các vấn đề:

### Build

Build hệ thống bình thường mất khoảng:

> **~30 phút**

### Start

Khi developer chạy application ở development mode, một số máy có thể consume lượng memory rất lớn và gặp:

```text
JavaScript heap out of memory
```

### Minimal run

NBE đã có cơ chế chạy application với một routing configuration tối giản, chỉ đưa một số module cần thiết vào development scope.

Cơ chế này đã giúp:

- giảm thời gian start
- giảm thời gian compile
- giảm bundle size
- giảm memory consumption

Tuy nhiên, **một số máy vẫn có thể gặp Heap OOM ngay khi start application**.

---

# 2. Mục tiêu của Phase 1

Hãy trả lời 5 câu hỏi chính:

### Câu hỏi 1

**Normal start của NBE đang phải xử lý những gì?**

### Câu hỏi 2

**Tại sao normal start có thể consume rất nhiều memory hoặc bị Heap OOM?**

### Câu hỏi 3

**Tại sao build NBE mất khoảng 30 phút?**

### Câu hỏi 4

**Tại sao minimal run cải thiện đáng kể performance?**

### Câu hỏi 5

**Tại sao minimal run vẫn có thể bị Heap OOM trên một số máy?**

---

# 3. Phạm vi điều tra

## 3.1. Build configuration

Kiểm tra các configuration liên quan, bao gồm nhưng không giới hạn:

```text
angular.json
tsconfig.json
tsconfig.*.json
package.json
```

và:

- build configuration
- development configuration
- production configuration
- file replacement
- source map
- AOT
- optimization
- cache
- build scripts
- start scripts
- environment configuration

Xác định configuration nào có khả năng ảnh hưởng đến:

- build time
- start time
- memory consumption

Không được kết luận configuration là root cause nếu chưa có evidence.

---

# 4. Phân tích Normal Start

Điều tra quá trình:

```text
npm start / ng serve
        ↓
Angular CLI
        ↓
Application entry
        ↓
Root configuration
        ↓
Routing
        ↓
Compilation
        ↓
Development server
```

Xác định:

- Application entry point
- Root module/configuration
- Root routing
- Những module/feature được đưa vào compilation scope
- Lazy-loaded routes
- Eager-loaded routes
- Những phần code được Angular xử lý ngay khi start

Đặc biệt quan tâm:

> Khi developer chưa truy cập bất kỳ feature nào, Angular đã phải compile/process những gì?

---

# 5. Phân tích Minimal Run

Tìm hiểu chính xác cơ chế minimal run hiện tại.

Xác định:

- Minimal routing nằm ở đâu
- Cách nó được kích hoạt
- File replacement có được sử dụng không
- Những module nào được đưa vào minimal scope
- Những module nào bị loại khỏi scope
- Dependency graph của minimal run khác normal run như thế nào

So sánh:

```text
Normal
vs
Minimal
```

về:

- Compilation scope
- Module count
- Start time
- Build time
- Memory consumption
- Output/bundle nếu có thể đo

---

# 6. Phân tích Heap OOM

Đây là phần **ưu tiên cao nhất** của phase này.

Phân biệt rõ hai trường hợp:

### A. Build Heap OOM

```text
ng build
    ↓
Memory tăng
    ↓
Heap OOM
```

### B. Start Heap OOM

```text
ng serve / npm start
    ↓
Application compilation
    ↓
Memory tăng
    ↓
Heap OOM
```

Không được mặc định rằng hai trường hợp này có cùng nguyên nhân.

Hãy xác định:

- OOM xảy ra ở bước nào
- Memory tăng trong quá trình nào
- Compilation scope tại thời điểm đó
- Configuration liên quan
- Có dấu hiệu dependency graph quá lớn hay không
- Có dấu hiệu source map hoặc build configuration ảnh hưởng không
- Có sự khác biệt giữa normal và minimal không

Nếu không thể xác định chính xác nguyên nhân, hãy ghi:

> **Chưa đủ evidence để xác nhận root cause.**

---

# 7. Memory Analysis

Nếu môi trường cho phép, hãy thực hiện các phép đo cần thiết.

Quan tâm đến:

```text
Peak Node heap
Peak RSS
Start memory
Build memory
```

Nếu có thể, hãy so sánh:

```text
Normal start
vs
Minimal start
```

và:

```text
Normal build
vs
Minimal build
```

Không được tự tạo số liệu.

Nếu một metric không thể đo trong môi trường hiện tại, ghi rõ:

```text
Chưa thể đo
```

và giải thích cách developer có thể đo metric đó sau này.

---

# 8. Build Performance Analysis

Phân tích tại sao build có thể mất khoảng 30 phút.

Không chấp nhận kết luận chung chung:

> "Project có hàng trăm module nên build chậm."

Cần tìm hiểu:

- Compilation scope
- Số lượng source file/module được xử lý
- Configuration
- Build pipeline
- TypeScript compilation
- Angular compilation
- Dependency resolution
- Source map
- Cache
- Optimization
- Các bước khác trong build pipeline

Nếu có thể xác định bottleneck cụ thể, hãy chỉ rõ:

```text
Bottleneck
    ↓
Nguyên nhân
    ↓
Evidence
    ↓
Ảnh hưởng
```

---

# 9. Normal vs Minimal

Tạo một bảng so sánh:

| Metric              | Normal | Minimal |
| ------------------- | -----: | ------: |
| Start time          |      ? |       ? |
| Build time          |      ? |       ? |
| Peak memory         |      ? |       ? |
| Initial bundle      |      ? |       ? |
| Total JS            |      ? |       ? |
| Module/source scope |      ? |       ? |

Chỉ điền số liệu đã thực sự đo được.

---

# 10. Phân loại kết quả

Mỗi finding phải được phân loại thành một trong ba loại:

### Confirmed

Đã có evidence đủ mạnh để kết luận.

### Likely

Có evidence đáng kể nhưng cần điều tra thêm để xác nhận.

### Unknown

Chưa đủ dữ liệu.

Ví dụ:

```text
[Confirmed]
Normal run có compilation scope lớn hơn minimal run.

Evidence:
...

Impact:
...

[Likely]
Shared dependency có thể làm compilation scope tăng đáng kể.

Evidence:
...

Cần kiểm tra thêm:
...
```

---

# 11. Không làm trong Phase này

**Tuyệt đối chưa thực hiện:**

- Refactor code
- Split SharedModule
- Migration NgModule → Standalone
- Thay đổi routing architecture
- Thay đổi dependency architecture
- Remove third-party library
- Migration Angular Material
- Rewrite application architecture
- Tạo implementation PR

Các vấn đề trên chỉ được **ghi nhận nếu phát hiện**, chưa xử lý.

---

# 12. Không sử dụng các giải pháp che giấu vấn đề

Không coi việc tăng:

```bash
NODE_OPTIONS=--max-old-space-size=8192
```

hoặc một giá trị heap lớn hơn là root-cause solution.

Nếu đề cập đến việc tăng heap, chỉ xem đây là:

> Temporary mitigation

và phải phân biệt rõ với nguyên nhân thực tế.

---

# 13. Output

Tạo một báo cáo:

# NBE Build, Start & Memory Performance Audit

## 1. Executive Summary

Tóm tắt tình trạng hiện tại.

## 2. Performance Baseline

Các metric đã đo được.

## 3. Normal Start Analysis

Angular đang xử lý những gì khi start bình thường?

## 4. Minimal Start Analysis

Minimal run khác normal run như thế nào?

## 5. Build Performance Analysis

Những yếu tố ảnh hưởng build time.

## 6. Memory Analysis

Memory tiêu thụ ở đâu và khi nào.

## 7. Heap OOM Analysis

Phân biệt:

- Start OOM
- Build OOM

và phân tích nguyên nhân của từng trường hợp.

## 8. Normal vs Minimal Comparison

Bảng so sánh.

## 9. Findings

Phân loại:

- Confirmed
- Likely
- Unknown

## 10. Evidence

Mỗi finding phải chỉ rõ:

- File
- Configuration
- Module
- Script
- Metric
- Hoặc evidence khác

## 11. Questions for Phase 2

Đưa ra **tối đa 5 vấn đề cần điều tra tiếp** ở Phase 2.

---

# Nguyên tắc quan trọng

Trong toàn bộ phase này:

> **Measure → Observe → Analyze → Identify Root Cause**

Không:

> **Observe → Assume → Refactor**

Nếu chưa đủ dữ liệu để kết luận, hãy nói rõ:

> **Chưa đủ evidence.**

Deliverable duy nhất của phase này là:

**NBE Build, Start & Memory Performance Audit**

Không implementation.
