# VAI TRÒ

Bạn đóng vai trò **Software Architect và Architecture Analyst** cho dự án NBE Angular.

Nhiệm vụ của bạn trong task này là:

1. Phân tích kiến trúc hiện tại của NBE.
2. Xác định các vấn đề kiến trúc hiện tại.
3. Phân tích dependency graph và các dependency hub.
4. Đề xuất và formalize kiến trúc mục tiêu cho giai đoạn phát triển mới.
5. Tạo bộ tài liệu kiến trúc làm source of truth cho các công việc phát triển NBE trong tương lai.

**Bạn KHÔNG phải implementation agent trong task này.**

Không tự ý refactor source code.

Không tự ý migrate module.

Không tự ý thay đổi kiến trúc hiện tại.

Trước tiên phải **đọc, phân tích repository và tạo tài liệu kiến trúc**.

---

# 1. BỐI CẢNH DỰ ÁN

NBE là một Angular application rất lớn.

Project đã trải qua quá trình migration từ Angular 8 lên các phiên bản Angular mới hơn.

Hiện tại project có:

- Hàng trăm Angular module.
- Nhiều NgModule legacy.
- Các Angular Material legacy module.
- Một `SharedModule` lớn.
- Một số shared dependency có khả năng kéo theo rất nhiều dependency khác.
- Nhiều dependency chéo giữa các module.
- Một dependency graph lớn.
- Một số developer machine gặp lỗi:

```text
JavaScript heap out of memory
```

khi chạy development application.

---

# 2. THAY ĐỔI MÔ HÌNH PHÁT TRIỂN

Trước đây NBE được phát triển theo mô hình tương đối tập trung.

Hiện tại development đã được phân chia cho các Scrum Team.

Mỗi Scrum Team phụ trách một business scope/domain khác nhau.

Ví dụ:

```text
Team A → Customer
Team B → Contract
Team C → Payment
```

Tuy nhiên ownership có thể thay đổi.

Ví dụ sau này:

```text
Team A → Contract
Team B → Customer
Team C → Payment
```

Việc ownership thay đổi **không được yêu cầu di chuyển source code**.

Do đó phải phân biệt rõ:

```text
Business Domain
        ≠
Scrum Team
```

Business domain là một boundary tương đối ổn định của source code.

Scrum Team là ownership hiện tại của domain.

---

# 3. MỤC TIÊU KIẾN TRÚC

Kiến trúc mới phải đáp ứng:

1. Giữ cho legacy code hiện tại tiếp tục hoạt động.
2. Không yêu cầu rewrite toàn bộ application.
3. Code mới phải tuân theo kiến trúc mới.
4. Business domain phải có boundary rõ ràng.
5. Giảm dependency chéo không cần thiết.
6. Giảm kích thước dependency graph.
7. Giảm compilation scope khi development.
8. Giảm memory consumption khi chạy Angular development.
9. Cho phép mỗi Scrum Team có development scope riêng.
10. Ownership thay đổi không làm source code phải di chuyển.
11. Tạo nền tảng để sử dụng Standalone architecture cho code mới.
12. Cho phép migration legacy từng bước khi có giá trị thực tế.

---

# 4. KHÔNG ĐƯỢC HIỂU MỤC TIÊU LÀ REWRITE

Không được mặc định đề xuất:

- Rewrite toàn bộ NBE.
- Migrate toàn bộ NgModule sang Standalone ngay lập tức.
- Xóa toàn bộ legacy code.
- Chuyển toàn bộ source code vào team folder.
- Micro Frontend.
- Tách thành nhiều Angular application.
- Nx chỉ vì project lớn.

Đây KHÔNG phải mục tiêu hiện tại.

Chiến lược là:

```text
Legacy hiện tại
      │
      └── tiếp tục hoạt động

Code mới
      │
      └── sử dụng kiến trúc mới

Legacy có vấn đề
      │
      └── refactor/migrate từng phần khi có giá trị
```

Ưu tiên là **giảm dependency graph và thiết lập boundary tốt**, không phải đạt một tỷ lệ migration nào đó.

---

# 5. SOURCE CODE ORGANIZATION

Kiến trúc mục tiêu có thể có dạng khái niệm:

```text
src/app/

├── core/
│
├── shared/
│
├── domains/
│   ├── customer/
│   ├── contract/
│   ├── payment/
│   └── ...
│
├── legacy/
│
└── development/
```

Đây chỉ là target concept.

Trước khi quyết định tên folder hoặc cấu trúc cụ thể, phải kiểm tra repository hiện tại.

Không được tự ý áp dụng cấu trúc này vào source code trong task này.

---

# 6. DOMAIN KHÔNG PHẢI TEAM

Đây là một nguyên tắc bắt buộc.

Source code phải tổ chức theo business domain/capability.

Ví dụ:

```text
domains/
├── customer/
├── contract/
└── payment/
```

Không tổ chức source code chính theo:

```text
team-a/
team-b/
team-c/
```

Không tạo:

```text
team-a/customer/
team-b/contract/
```

chỉ vì hiện tại Team A hoặc Team B đang ownership domain đó.

Ví dụ:

```text
domains/customer/
```

vẫn tồn tại khi:

```text
Team A → Customer
```

và cũng vẫn tồn tại khi:

```text
Team B → Customer
```

Khi ownership thay đổi, chỉ thay đổi ownership/development configuration.

Source code không di chuyển.

---

# 7. DEVELOPMENT SCOPE

NBE hiện tại có một development mode gọi là `minimal`.

Minimal routing hiện tại hoạt động bằng cách giới hạn các module/feature được khai báo trong routing.

Ví dụ application bình thường:

```text
Application
├── Feature A
├── Feature B
├── Feature C
├── Feature D
├── Feature E
└── ...
```

Minimal:

```text
Application
├── Feature A
└── Feature B
```

Trong thực tế của NBE, việc này đã làm giảm đáng kể:

- Compilation scope.
- Bundle size.
- Development startup time.
- Memory consumption.

Do đó:

> Minimal routing phải được xem là một cơ chế kiến trúc để giới hạn development compilation scope, không chỉ là workaround cho máy yếu.

---

# 8. TEAM-SPECIFIC DEVELOPMENT ROUTING

Kiến trúc mục tiêu là mở rộng ý tưởng minimal thành development scope cho từng team.

Ví dụ:

```text
development/
├── team-a-routing.module.ts
├── team-b-routing.module.ts
├── team-c-routing.module.ts
└── ...
```

Team A:

```text
Team A
├── Customer
└── Contract
```

Team B:

```text
Team B
├── Payment
└── Invoice
```

Mỗi team routing chỉ khai báo các domain/module cần thiết cho development của team đó.

Mục tiêu:

```text
Team A development
        ↓
chỉ compile scope cần thiết

Team B development
        ↓
chỉ compile scope cần thiết
```

---

# 9. ANGULAR.JSON FILE REPLACEMENT

Application vẫn có một routing entry point chung.

Ví dụ:

```text
src/app/app-routing.module.ts
```

Các development configuration có thể sử dụng Angular `fileReplacements`.

Khái niệm:

```text
app-routing.module.ts
        │
        ├── Team A configuration
        │
        ├── Team B configuration
        │
        └── Team C configuration
```

Ví dụ concept:

```json
{
  "configurations": {
    "team-a": {
      "fileReplacements": [
        {
          "replace": "src/app/app-routing.module.ts",
          "with": "src/app/development/team-a-routing.module.ts"
        }
      ]
    }
  }
}
```

Sau đó:

```bash
ng serve --configuration=team-a
```

sẽ sử dụng routing configuration của Team A.

Tương tự:

```bash
ng serve --configuration=team-b
```

sẽ sử dụng routing configuration của Team B.

**Phải kiểm tra implementation hiện tại của repository trước khi kết luận chính xác cách cấu hình.**

Không được giả định repository đã sử dụng chính xác cấu trúc trên.

---

# 10. DEVELOPMENT ROUTING KHÔNG PHẢI SOURCE OWNERSHIP

Đây là nguyên tắc quan trọng.

Team routing chỉ biểu diễn:

> "Team này hiện tại cần development scope nào?"

Nó KHÔNG biểu diễn:

> "Source code này thuộc folder của team nào?"

Ví dụ:

```text
domains/
├── customer/
├── contract/
└── payment/
```

Team A:

```text
team-a-routing
├── customer
└── contract
```

Team B:

```text
team-b-routing
└── payment
```

Nếu Contract chuyển sang Team B:

```text
team-a-routing
└── customer

team-b-routing
├── contract
└── payment
```

Nhưng source code vẫn:

```text
domains/contract/
```

Không được move source code.

---

# 11. DEPENDENCY GRAPH — VẤN ĐỀ TRỌNG TÂM

Một vấn đề lớn hiện tại của NBE có khả năng nằm ở dependency graph.

Đặc biệt cần điều tra:

```text
Feature
   ↓
SharedModule
   ↓
nhiều shared dependency
   ↓
Legacy module
   ↓
large legacy graph
```

Feature có thể chỉ cần một vài component hoặc directive.

Nhưng vì import một `SharedModule` lớn, feature có thể vô tình kéo theo rất nhiều dependency không liên quan.

Điều này có thể gây:

- Compilation graph lớn.
- Bundle lớn.
- Startup chậm.
- Node.js memory consumption cao.
- Cross-domain coupling.
- Khó kiểm soát dependency.

Ngoài `SharedModule`, phải tìm các dependency hub khác có hành vi tương tự.

---

# 12. SHARED MODULE — PHẢI PHÂN TÍCH TRƯỚC KHI REFACTOR

`SharedModule` là một đối tượng cần ưu tiên điều tra.

Không được mặc định rằng tất cả nội dung trong `SharedModule` đều nên nằm trong `shared`.

Phải phân loại từng dependency.

Ví dụ:

```text
SharedModule
├── Angular primitive
├── Shared UI component
├── Directive
├── Pipe
├── Utility
├── Business-specific component
├── Legacy module
└── Other shared dependency
```

Phân loại thành các nhóm phù hợp như:

```text
core/
shared/
domains/<domain>/
legacy/
```

hoặc import trực tiếp nếu phù hợp.

Mục tiêu:

> Một feature không nên import một aggregate module lớn nếu nó chỉ cần một phần nhỏ chức năng.

---

# 13. MỤC TIÊU CỦA VIỆC TÁCH SHAREDMODULE

Không đơn giản là:

```text
SharedModule
        ↓
nhiều module nhỏ
```

Mà mục tiêu là:

```text
Feature A
 ├── dependency A
 ├── dependency B
 └── dependency C

Feature B
 ├── dependency D
 └── dependency E
```

thay vì:

```text
Feature A ──┐
Feature B ──┼── SharedModule ──> huge graph
Feature C ──┘
```

Dependency phải trở nên explicit và có boundary.

---

# 14. LEGACY STRATEGY

Legacy code vẫn là một phần hợp lệ của NBE trong giai đoạn chuyển tiếp.

Không được migrate legacy chỉ vì muốn kiến trúc đẹp.

Legacy chỉ nên được refactor/migrate khi:

- Có dependency problem.
- Gây compilation problem.
- Gây coupling lớn.
- Cản trở domain boundary.
- Đang được sửa đổi đáng kể.
- Migration mang lại giá trị rõ ràng.

Chiến lược:

```text
Existing legacy
      │
      └── giữ ổn định

New development
      │
      └── Standalone

Problematic legacy
      │
      └── migrate/refactor từng phần
```

---

# 15. STANDALONE STRATEGY

Code Angular mới phải ưu tiên Standalone architecture.

Không tạo NgModule mới chỉ để tiếp tục pattern legacy.

Tuy nhiên:

> Không yêu cầu migrate toàn bộ code hiện tại sang Standalone.

Standalone được sử dụng để tạo:

- Explicit imports.
- Dependency boundaries rõ ràng.
- Domain isolation.
- Smaller dependency graph.
- Better development compilation scope.

Không sử dụng "số lượng Standalone component" làm KPI chính.

KPI chính là:

> **Dependency graph của một development scope phải nhỏ, explicit và predictable.**

---

# 16. MỐI QUAN HỆ GIỮA ROUTING VÀ DEPENDENCY CLEANUP

Hai hướng này phải được xem là hai cơ chế bổ trợ nhau.

```text
                 Development Performance
                          │
              ┌───────────┴───────────┐
              │                       │
      Development Routing       Dependency Cleanup
              │                       │
       giới hạn scope           giảm dependency
              │                       │
              └───────────┬───────────┘
                          ↓
                  Smaller compile graph
                          ↓
                    Less memory
                          ↓
                   Faster startup
```

Routing giới hạn graph từ entry point.

Dependency cleanup làm graph bên trong nhỏ hơn.

Nếu chỉ làm routing:

```text
Team A
  ↓
SharedModule
  ↓
huge legacy graph
```

thì minimal vẫn có thể rất lớn.

Nếu chỉ tách SharedModule nhưng không có development scope:

```text
Application
  ↓
nhiều domain
  ↓
compile graph vẫn lớn
```

Do đó cần kết hợp cả hai.

---

# 17. NHIỆM VỤ CỦA BẠN

Trước khi đưa ra recommendation, hãy inspect repository.

Phải kiểm tra tối thiểu:

1. Application entry point.
2. `app-routing.module`.
3. Các routing module hiện tại.
4. `angular.json`.
5. Các Angular build configurations.
6. Configuration của `minimal` hiện tại.
7. `package.json` và các development scripts.
8. `SharedModule`.
9. Các shared module khác.
10. Các legacy module lớn.
11. Lazy-loaded routes.
12. Standalone components hiện có.
13. Các domain/module boundary hiện tại.
14. Các dependency chéo.
15. Các dependency hub có fan-out lớn.

**Không được thay đổi source code trong bước này.**

---

# 18. CÁC DOCUMENT CẦN TẠO

Tạo bộ tài liệu architecture dưới một thư mục phù hợp với convention hiện tại của repository.

Nếu repository chưa có convention, có thể sử dụng:

```text
docs/
└── architecture/
    ├── architecture-overview.md
    ├── target-architecture.md
    ├── domain-boundaries.md
    ├── development-scope.md
    ├── routing-architecture.md
    ├── dependency-management.md
    ├── shared-module-strategy.md
    ├── legacy-strategy.md
    ├── standalone-strategy.md
    └── migration-roadmap.md
```

Có thể điều chỉnh tên file nếu repository đã có convention khác.

---

# 19. NỘI DUNG TỪNG DOCUMENT

## architecture-overview.md

Mô tả:

- Kiến trúc hiện tại.
- Vấn đề hiện tại.
- Kiến trúc mục tiêu.
- Legacy/new coexistence.
- Domain.
- Development scope.
- Dependency strategy.

Sử dụng Mermaid khi phù hợp.

---

## target-architecture.md

Mô tả:

- `core`
- `shared`
- `domains`
- `legacy`
- `development`
- Dependency direction.
- Allowed dependencies.
- Discouraged dependencies.

Phải có rule rõ ràng.

---

## domain-boundaries.md

Mô tả:

```text
Domain != Team
```

Giải thích:

- Source code thuộc domain.
- Team là ownership.
- Ownership thay đổi không được yêu cầu source movement.

Đưa ví dụ cụ thể.

---

## development-scope.md

Mô tả:

```text
Full application
Team A scope
Team B scope
Team C scope
```

Giải thích tại sao development scope tồn tại.

Giải thích nó giúp giảm compilation graph như thế nào.

---

## routing-architecture.md

Mô tả quan hệ giữa:

```text
app-routing.module.ts
team-a-routing.module.ts
team-b-routing.module.ts
angular.json
fileReplacements
ng serve configuration
```

Phân biệt rõ:

- Production routing.
- Full development routing.
- Team-specific development routing.

Không đưa ra implementation detail nếu chưa xác minh trong repository.

---

## dependency-management.md

Đây là document quan trọng.

Phải định nghĩa:

- Dependency boundary.
- Transitive dependency.
- Dependency hub.
- Cross-domain dependency.
- Aggregate module.
- Shared dependency.
- Quy tắc cho code mới.
- Quy tắc tránh dependency graph phình trở lại.

Giải thích quan hệ giữa dependency graph và compilation/memory.

---

## shared-module-strategy.md

Phân tích `SharedModule` hiện tại.

Document phải chỉ ra:

- Nó chứa những gì.
- Thành phần nào thực sự generic.
- Thành phần nào thuộc domain.
- Thành phần nào thuộc legacy.
- Dependency nào tạo graph lớn.
- Đề xuất boundary trong tương lai.

**Chưa được refactor source code.**

---

## legacy-strategy.md

Định nghĩa:

- Legacy được giữ ở đâu.
- Code mới không được làm gì.
- Khi nào legacy nên được migrate.
- Legacy và Standalone coexist như thế nào.
- Cách tránh code mới tiếp tục làm legacy graph lớn hơn.

---

## standalone-strategy.md

Định nghĩa:

- Rule cho code mới.
- Import strategy.
- Shared component strategy.
- Legacy integration.
- Migration strategy.

Không đề xuất mass migration.

---

## migration-roadmap.md

Tạo roadmap theo mức độ ưu tiên và architectural leverage.

Định hướng mong muốn:

```text
Phase 1
Phân tích dependency graph
        ↓
Phase 2
Phân tích SharedModule và dependency hubs
        ↓
Phase 3
Loại bỏ dependency không cần thiết
        ↓
Phase 4
Formalize development/team routing scopes
        ↓
Phase 5
Thiết lập domain boundaries
        ↓
Phase 6
Code mới sử dụng Standalone
        ↓
Phase 7
Migrate legacy có chọn lọc
```

Không được mặc định rằng toàn bộ legacy module phải migrate.

---

# 20. CÁC CÂU HỎI PHẢI TRẢ LỜI

Trong quá trình phân tích repository, phải trả lời rõ:

1. Dependency hub lớn nhất hiện tại là gì?
2. `SharedModule` đang kéo theo những dependency nào?
3. Dependency nào đang bị kéo vào nhiều domain không liên quan?
4. Những legacy module nào nằm trên dependency path của minimal routing?
5. Shared component nào có thể được tách độc lập?
6. Business component nào đang nằm sai trong shared?
7. Domain nào đang phụ thuộc chéo mạnh?
8. Minimal routing hiện tại có thể mở rộng thành team development scope không?
9. `angular.json` hiện tại nên tổ chức configuration như thế nào?
10. Làm thế nào để architecture mới không tạo lại một `SharedModule` thứ hai?
11. Dependency boundary nào cần được enforce cho code mới?
12. Những phần nào nên migrate trước nếu mục tiêu là giảm compilation/memory?

---

# 21. CÁC ĐIỀU KHÔNG ĐƯỢC LÀM

Trong task này:

- Không sửa application source code.
- Không migrate module.
- Không tạo module mới.
- Không refactor `SharedModule`.
- Không di chuyển source code.
- Không introduce Nx.
- Không introduce Micro Frontend.
- Không tách application.
- Không rename hàng loạt file.
- Không rewrite legacy.

Nếu phát hiện một thay đổi cần thực hiện:

> Chỉ document nó như một proposed change.

Không tự động implement.

---

# 22. YÊU CẦU CHẤT LƯỢNG DOCUMENT

Documents phải đủ rõ để:

- Developer mới đọc được.
- Architect có thể review.
- AI coding agent có thể sử dụng làm source of truth.
- Các quyết định kiến trúc trong tương lai có thể dựa vào documents này.

Không sử dụng các câu quá chung chung như:

> "Giữ module loosely coupled."

Thay vào đó phải đưa ra rule cụ thể.

Ví dụ:

> Domain không được import trực tiếp implementation nội bộ của domain khác. Nếu functionality cần được sử dụng bởi nhiều domain, phải xác định rõ shared boundary hoặc public API phù hợp.

Mỗi architectural rule nên có:

- Rule.
- Rationale.
- Ví dụ đúng.
- Anti-pattern.
- Migration guidance nếu cần.

Sử dụng Mermaid cho các dependency/routing diagram quan trọng.

---

# 23. ARCHITECTURE DECISION SUMMARY

Cuối cùng, tạo một phần:

# Architecture Decision Summary

Tóm tắt các nguyên tắc quan trọng nhất mà mọi development mới của NBE phải tuân thủ.

Đặc biệt phải bao gồm:

1. Domain không phải Team.
2. Source code không tổ chức theo Scrum Team.
3. Team routing chỉ là development scope.
4. `angular.json` có thể được sử dụng để chọn development routing configuration.
5. Development scope phải giới hạn compilation graph.
6. Không sử dụng một aggregate `SharedModule` lớn để chứa mọi thứ.
7. Shared dependency phải có boundary rõ ràng.
8. Business logic không được đưa vào generic shared.
9. Code mới ưu tiên Standalone.
10. Không tạo thêm legacy architecture cho code mới.
11. Legacy được migrate từng bước, không rewrite toàn bộ.
12. Dependency graph size là một architectural concern.
13. Mọi architectural change phải ưu tiên giảm coupling và giữ source ownership độc lập với team ownership.

---

# QUY TRÌNH THỰC HIỆN

Thực hiện theo thứ tự:

```text
1. Inspect repository
        ↓
2. Hiểu architecture hiện tại
        ↓
3. Phân tích routing hiện tại
        ↓
4. Phân tích SharedModule
        ↓
5. Phân tích dependency hubs
        ↓
6. Xác định domain boundaries
        ↓
7. Đánh giá development scope hiện tại
        ↓
8. Thiết kế target architecture
        ↓
9. Tạo architecture documents
        ↓
10. Tạo Architecture Decision Summary
```

Không được bỏ qua bước phân tích repository.

Không được tạo documentation chỉ dựa trên prompt này.

Các quyết định cuối cùng phải phản ánh **kiến trúc thực tế của repository**.

Nếu phát hiện rằng một giả định trong prompt không phù hợp với repository hiện tại, hãy:

1. Ghi nhận sự khác biệt.
2. Giải thích nguyên nhân.
3. Đề xuất cách điều chỉnh.
4. Không tự ý sửa source code.
