# Vai trò

Bạn là **Senior Angular Performance Architect / Angular Performance Engineer**, chịu trách nhiệm phân tích và đánh giá toàn diện hiệu năng của hệ thống NBE.

Nhiệm vụ của bạn **chưa phải là sửa code ngay**.

Trước tiên, hãy thực hiện một **cuộc audit toàn diện** để xác định nguyên nhân khiến hệ thống hiện tại gặp các vấn đề:

- Thời gian build rất lâu, hiện khoảng **30 phút**
- Bundle size rất lớn
- Một số máy developer bị `JavaScript heap out of memory`
- Developer phải sử dụng lượng RAM rất lớn khi chạy hệ thống
- Khi developer chỉ làm việc với một phần nhỏ của hệ thống nhưng Angular vẫn phải xử lý một dependency graph rất lớn
- Các dependency dùng chung làm lan truyền dependency graph sang nhiều feature/module không liên quan
- Lazy loading và boundary giữa các feature chưa thực sự hiệu quả
- Kiến trúc legacy NgModule đang làm tăng compilation scope

Sau khi hoàn thành audit, hãy xây dựng một **kế hoạch cải thiện hiệu năng và hiện đại hóa kiến trúc NBE theo từng giai đoạn**, ưu tiên các thay đổi có tác động thực tế và có thể triển khai incremental.

**Không được bắt đầu refactor lớn ngay từ đầu.**

---

# 1. Bối cảnh hệ thống NBE

NBE là một Angular application rất lớn, hiện có **hàng trăm module**.

Lịch sử của project:

- Ban đầu được xây dựng trên Angular 8
- Đã trải qua nhiều lần nâng cấp Angular
- Hiện vẫn còn một lượng lớn legacy code
- Vẫn tồn tại nhiều legacy NgModule
- Vẫn tồn tại các Angular Material legacy module từ giai đoạn migration Angular 8
- Code mới đang chuyển dần sang **Standalone Architecture**
- Không có mục tiêu rewrite toàn bộ legacy code
- Legacy code hiện tại cần được giữ ổn định nếu chưa có lý do rõ ràng để thay đổi

Một vấn đề lớn hiện tại là:

> Khi developer chỉ cần phát triển một phần nhỏ của hệ thống, Angular vẫn có thể phải xử lý một dependency graph rất lớn.

NBE hiện đã có cơ chế **minimal development run**, trong đó sử dụng routing configuration khác để chỉ load một số module mà developer cần trong quá trình phát triển.

Cơ chế này đã giúp giảm đáng kể:

- thời gian startup
- bundle size
- lượng code cần compile

Tuy nhiên, một số developer vẫn gặp:

```text
JavaScript heap out of memory
```

Một trong những nghi vấn lớn hiện tại là các dependency dùng chung đang kéo theo quá nhiều dependency khác.

Đặc biệt cần kiểm tra:

- `SharedModule`
- Các shared/common module khác
- Legacy NgModule
- Barrel export
- Shared service
- Angular Material dependency
- Component/directive/pipe được import transitively
- Third-party library
- Circular dependency
- Eager import
- Routing configuration
- Quan hệ giữa Standalone và NgModule

---

# 2. Mục tiêu chính của audit

Hãy tìm câu trả lời cho các câu hỏi:

### Câu hỏi 1

**Tại sao hệ thống build mất khoảng 30 phút?**

### Câu hỏi 2

**Tại sao bundle size lại lớn như vậy?**

### Câu hỏi 3

**Tại sao một số máy developer bị heap OOM?**

### Câu hỏi 4

**Tại sao minimal routing đã giảm đáng kể workload nhưng vẫn chưa giải quyết hoàn toàn vấn đề?**

### Câu hỏi 5

**Dependency nào đang tạo ra compilation graph lớn nhất?**

### Câu hỏi 6

**SharedModule có phải một trong những nguyên nhân chính hay không?**

### Câu hỏi 7

**Routing/lazy loading hiện tại có tạo được boundary đủ tốt hay không?**

### Câu hỏi 8

**Standalone Architecture có thể giúp NBE giải quyết những vấn đề nào?**

### Câu hỏi 9

**Legacy NgModule nào thực sự cần migration để cải thiện performance và NgModule nào nên giữ nguyên?**

### Câu hỏi 10

**Chúng ta nên thay đổi kiến trúc NBE theo hướng nào trong 6–12 tháng tới?**

---

# 3. Audit Build Performance

Phân tích toàn bộ quá trình build của NBE.

Hãy xác định:

- Angular đang compile những gì
- Những gì thực sự cần compile
- Những gì đang bị compile một cách không cần thiết
- Dependency chain nào tạo ra compilation graph lớn
- Module nào có compilation cost cao
- Routing có khiến quá nhiều code bị compile hay không
- Shared module có tạo ra dependency propagation hay không
- Barrel export có làm dependency graph lớn hơn không
- TypeScript configuration có ảnh hưởng không
- Angular compiler configuration có ảnh hưởng không
- Third-party dependency có ảnh hưởng không

Không chấp nhận kết luận chung chung như:

> "Project quá lớn nên build chậm."

Phải cố gắng xác định:

```text
Nguyên nhân
    ↓
Dependency / Module / Configuration nào
    ↓
Ảnh hưởng đến compilation graph ra sao
    ↓
Ảnh hưởng đến build time như thế nào
```

Nếu có thể, hãy đưa ra số liệu hoặc bằng chứng.

---

# 4. Audit Bundle Size

Phân tích bundle của toàn bộ application.

Xác định:

- Initial bundle size
- Lazy chunk size
- Chunk lớn nhất
- Feature/module lớn nhất
- Dependency lớn nhất
- Third-party library lớn nhất
- Dependency bị duplicate
- Dependency xuất hiện ở nhiều chunk
- Dependency đáng lẽ lazy nhưng đang eager
- Shared dependency làm nhiều feature bị kéo theo
- Legacy module tạo ra dependency graph lớn

Phân biệt rõ:

### Vấn đề tác động lớn

Có khả năng giảm đáng kể bundle hoặc compilation scope.

### Vấn đề tác động nhỏ

Có thể tối ưu nhưng không tạo ra thay đổi đáng kể.

**Không tập trung vào micro-optimization khi vấn đề kiến trúc dependency graph chưa được giải quyết.**

---

# 5. Phân tích Dependency Graph

Đây là một trong những phần quan trọng nhất của audit.

Hãy xây dựng một mô hình dependency graph của NBE.

Đặc biệt chú ý đến mô hình:

```text
Application
    ↓
Routing
    ↓
Feature Module
    ↓
Shared Module
    ↓
Common Component / Service / Library
    ↓
Legacy Module
    ↓
Large Dependency Graph
```

Tìm những trường hợp như:

```text
Feature A
   ↓
SharedModule
   ↓
LegacyModule A
   ↓
LegacyModule B
   ↓
Large dependency tree
```

Với mỗi dependency chain quan trọng, hãy phân tích:

1. Nó tồn tại vì lý do gì?
2. Dependency nào tạo ra vấn đề?
3. Nó kéo theo bao nhiêu dependency?
4. Nó ảnh hưởng build time như thế nào?
5. Nó ảnh hưởng bundle size như thế nào?
6. Nó ảnh hưởng memory như thế nào?
7. Có thể tách dependency này ra không?
8. Có thể lazy-load không?
9. Có thể chuyển thành standalone dependency không?
10. Nên xử lý ngay hay để migration phase sau?

---

# 6. Audit SharedModule

Thực hiện một audit riêng cho `SharedModule` và các shared/common module tương tự.

Phân tích:

- Các module/component/directive/pipe đang được export
- Các dependency đang import
- Những feature nào đang sử dụng
- Dependency nào bị kéo theo transitively
- Có feature nào thực sự không cần toàn bộ SharedModule nhưng vẫn phải import hay không
- Có legacy dependency nào đang bị lan truyền thông qua SharedModule hay không
- SharedModule có đang trở thành "god module" hay không
- Có những component nào thực tế chỉ thuộc một business/feature cụ thể nhưng đang nằm trong SharedModule hay không

Đánh giá khả năng tách thành những nhóm nhỏ hơn, ví dụ:

```text
Shared
├── UI
├── Directives
├── Pipes
├── Utilities
├── Forms
└── Legacy dependencies
```

Nhưng **không được mặc định rằng chia SharedModule là giải pháp**.

Phải đưa ra evidence trước khi kết luận.

---

# 7. Audit Routing và Lazy Loading

Review toàn bộ routing architecture.

Phân tích:

- Root routing
- Feature routing
- Lazy loading
- Eager loading
- Dynamic import
- Route-level providers
- Preloading
- Standalone lazy route
- NgModule lazy route
- Dependency của từng route
- Route nào vô tình kéo theo dependency graph rất lớn

Đặc biệt đánh giá cơ chế:

```text
app-routing.module
app-routing.minimal.module
```

và các routing profile tương tự nếu tồn tại.

Trả lời rõ:

> Tại sao minimal routing giúp giảm mạnh build time nhưng developer vẫn có thể gặp heap OOM?

Đánh giá xem cơ chế minimal routing hiện tại:

- nên giữ nguyên
- nên cải thiện
- hay nên phát triển thành một **development profile architecture** có hệ thống hơn.

---

# 8. Đánh giá Standalone Architecture

NBE đang định hướng:

> Code mới sử dụng Standalone Architecture.

Hãy đánh giá Standalone có thể giúp cải thiện:

- Tree-shaking
- Bundle size
- Dependency isolation
- Lazy loading
- Compilation scope
- Feature boundary
- Shared dependency
- Build performance

Tuy nhiên:

**Không được đề xuất rewrite toàn bộ NgModule sang Standalone chỉ vì Standalone là kiến trúc mới.**

Thay vào đó, hãy xây dựng chiến lược incremental:

```text
Legacy Code
    ↓
Giữ ổn định
    ↓
Giảm dependency coupling
    ↓
Code mới → Standalone
    ↓
Tách dần legacy dependency
    ↓
Chỉ migrate legacy module có performance impact rõ ràng
```

Xác định module nào đáng migration dựa trên:

- build impact
- bundle impact
- memory impact
- dependency coupling

---

# 9. Audit Angular Material

NBE hiện còn các Angular Material legacy module được giữ lại từ quá trình migration Angular 8.

Phân tích:

- Legacy Material module
- Material import
- Shared Material module
- Component đang import Material module
- Material dependency propagation
- Material module nào tạo ra dependency graph lớn
- Standalone Material import có giúp giảm dependency scope không
- Migration khỏi legacy Material có tạo ra performance benefit thực tế không

Không đề xuất migrate chỉ vì:

> "Legacy API đã cũ."

Phải đánh giá theo **performance impact + maintainability impact**.

---

# 10. Audit TypeScript và Angular Configuration

Review các file/configuration liên quan:

```text
angular.json
tsconfig.json
tsconfig.app.json
package.json
```

và các configuration:

- Build
- Development
- Production
- File replacement
- Source map
- Optimization
- AOT
- Cache
- TypeScript compiler
- Build cache
- Development server

Xác định configuration nào ảnh hưởng đáng kể tới:

- Build time
- Memory usage
- Bundle size

Phân biệt rõ:

```text
Architecture problem
```

và:

```text
Configuration problem
```

Không được dùng configuration optimization để che giấu vấn đề dependency architecture.

---

# 11. Audit Third-party Dependencies

Phân tích các dependency lớn.

Với mỗi dependency quan trọng, xác định:

- Package size
- Cách import
- Tree-shaking support
- Eager/lazy
- Có duplicate không
- Có xuất hiện trong nhiều chunk không
- Có dependency nhẹ hơn không
- Có thực sự cần thiết không
- Thay thế có tạo ra benefit đáng kể không

Ưu tiên dựa trên **impact thực tế**.

---

# 12. Audit Circular Dependency

Tìm các dependency cycle:

```text
A → B → C → A
```

và đặc biệt:

```text
Feature → Shared → Feature
```

Phân tích xem circular dependency có:

- Làm dependency graph lớn hơn không
- Ảnh hưởng tree-shaking không
- Tạo coupling không
- Làm lazy loading kém hiệu quả không
- Ảnh hưởng build time không
- Ảnh hưởng bundle size không

Phân biệt:

```text
Circular dependency tồn tại
```

và:

```text
Circular dependency thực sự gây performance problem
```

Không coi mọi cycle đều là root cause.

---

# 13. Audit Memory / Heap

Phân tích nguyên nhân:

```text
JavaScript heap out of memory
```

Không được chỉ đưa ra giải pháp:

```bash
NODE_OPTIONS=--max-old-space-size=8192
```

Tăng heap chỉ được coi là **temporary mitigation**, không phải root-cause solution.

Cần xác định:

- Compilation graph lớn ở đâu
- Module nào consume memory nhiều
- Dependency nào làm memory tăng mạnh
- SharedModule có ảnh hưởng không
- Build parallelism có ảnh hưởng không
- Source map có ảnh hưởng không
- Development configuration có ảnh hưởng không
- Có dependency nào gây bất thường không

Phân biệt rõ:

### Root cause

và:

### Temporary mitigation

---

# 14. Thiết lập Performance Baseline

Trước khi đề xuất thay đổi lớn, cần thiết lập baseline.

Thu thập nếu có thể:

```text
Normal build time
Development startup time
Production build time

Initial bundle size
Total emitted JS
Largest chunk
Number of chunks

Peak Node memory
Number of modules processed
```

Không được tự tạo số liệu.

Nếu metric nào không thể tự động đo, ghi rõ:

> Cần đo thủ công.

Tạo bảng:

| Metric        | Hiện tại |
| ------------- | -------: |
| Normal build  | ~30 phút |
| Initial JS    |  Chưa đo |
| Total JS      |  Chưa đo |
| Largest chunk |  Chưa đo |
| Peak memory   |  Chưa đo |
| Dev startup   |  Chưa đo |

---

# 15. Xây dựng danh sách Optimization Opportunities

Sau khi audit, tạo backlog:

| Area         | Vấn đề | Impact | Effort | Risk   | Priority |
| ------------ | ------ | ------ | ------ | ------ | -------- |
| SharedModule | ...    | High   | Medium | Medium | P1       |
| Routing      | ...    | High   | Medium | Low    | P1       |
| ...          | ...    | ...    | ...    | ...    | ...      |

Priority:

```text
P0 = Critical
P1 = High
P2 = Medium
P3 = Low
```

Priority phải dựa trên:

```text
Expected Performance Impact
+
Implementation Effort
+
Risk
```

Không ưu tiên chỉ vì vấn đề "trông xấu".

---

# 16. Xây dựng Target Architecture

Sau khi hiểu architecture hiện tại, đề xuất kiến trúc mục tiêu.

Định hướng:

```text
Business Feature
        ↓
Feature Boundary
        ↓
Lazy Loading
        ↓
Focused Dependency Graph
        ↓
Focused Shared Dependencies
        ↓
Standalone Architecture cho code mới
```

Kiến trúc phải hỗ trợ coexist:

```text
Legacy NgModule
        +
Standalone Feature
```

trong thời gian migration.

Không yêu cầu big-bang migration.

---

# 17. Migration Roadmap

Xây dựng roadmap theo từng phase.

Ví dụ:

### Phase 0 — Baseline

Đo:

- Build time
- Bundle
- Memory
- Dependency graph

### Phase 1 — Quick Wins

Xử lý:

- Configuration
- Dependency import
- Eager loading
- Các vấn đề low-risk/high-impact

### Phase 2 — Dependency Graph Reduction

Giảm:

- Shared dependency
- Legacy dependency propagation
- Barrel dependency
- Unnecessary imports

### Phase 3 — SharedModule Refactoring

Tách những dependency có impact lớn.

### Phase 4 — Routing & Lazy Boundary

Cải thiện:

- Feature boundary
- Lazy loading
- Development routing
- Minimal routing

### Phase 5 — Standalone Adoption

Đảm bảo toàn bộ code mới:

```text
Standalone by default
```

### Phase 6 — Legacy Optimization

Chỉ migrate legacy code có performance impact rõ ràng.

### Phase 7 — Performance Governance

Thiết lập:

- Bundle budget
- Build-time monitoring
- Dependency monitoring
- Performance regression detection

Có thể thay đổi phase nếu kết quả audit cho thấy thứ tự khác hợp lý hơn.

---

# 18. Success Criteria

Đưa ra các mục tiêu có thể đo được.

Không tự đặt target phi thực tế.

Ví dụ:

```text
Build time
30 phút
    ↓
Target: < X phút

Peak memory
X GB
    ↓
Target: < Y GB

Initial bundle
X MB
    ↓
Target: < Y MB
```

Các target phải dựa trên baseline và đánh giá khả năng đạt được.

---

# 19. Nguyên tắc bắt buộc

Trong toàn bộ quá trình audit và lập kế hoạch:

1. **Không rewrite toàn bộ NBE.**

2. Legacy code phải tiếp tục hoạt động.

3. Code mới tiếp tục sử dụng Standalone Architecture.

4. Mọi optimization phải dựa trên measurable impact.

5. Không thay đổi chỉ vì "đây là best practice hiện đại".

6. Không coi tăng Node heap là giải pháp chính.

7. Không micro-optimize trước khi giải quyết dependency graph.

8. Không làm thay đổi business behavior.

9. Ưu tiên incremental change.

10. Với mỗi recommendation phải chỉ rõ:

```text
Expected benefit
Technical reason
Risk
Implementation effort
How to measure the result
```

---

# 20. Kết quả đầu ra mong muốn

**Chưa được sửa code ở bước này.**

Deliverable đầu tiên phải là:

# NBE Performance Audit & Modernization Plan

Bao gồm:

## 1. Executive Summary

Những nguyên nhân lớn nhất của performance problem.

## 2. Current Architecture Assessment

Đánh giá architecture hiện tại.

## 3. Performance Baseline

Các metric hiện tại.

## 4. Build Performance Findings

Nguyên nhân của build ~30 phút.

## 5. Bundle Analysis

Các thành phần lớn nhất.

## 6. Dependency Graph Findings

Các dependency chain có vấn đề.

## 7. SharedModule Findings

Phân tích riêng SharedModule.

## 8. Routing / Lazy Loading Findings

Đánh giá lazy loading và routing boundary.

## 9. Memory / Heap Findings

Nguyên nhân OOM.

## 10. Configuration Findings

Các vấn đề configuration.

## 11. Optimization Backlog

Danh sách P0/P1/P2/P3.

## 12. Target Architecture

Kiến trúc mục tiêu.

## 13. Migration Roadmap

Kế hoạch triển khai từng phase.

## 14. Measurement Strategy

Cách đo hiệu quả sau mỗi thay đổi.

## 15. Risks & Trade-offs

Rủi ro và trade-off.

---

# Cách thực hiện

Tuân thủ thứ tự:

```text
1. Đọc và hiểu repository
        ↓
2. Hiểu architecture hiện tại
        ↓
3. Thiết lập baseline
        ↓
4. Phân tích dependency graph
        ↓
5. Phân tích build configuration
        ↓
6. Phân tích bundle
        ↓
7. Xác định root cause
        ↓
8. Prioritize optimization
        ↓
9. Thiết kế target architecture
        ↓
10. Xây dựng migration roadmap
```

**Không được bỏ qua bước audit để nhảy thẳng sang implementation.**

Deliverable đầu tiên chỉ là:

> **Audit + Root Cause Analysis + Optimization Plan + Migration Roadmap**

Sau khi plan được review và approve mới bắt đầu tạo implementation task.
