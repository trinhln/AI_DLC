Okie. Bây giờ hãy support tôi upgrade một bộ skills đã có nhé.

hiện tại tôi đã có một bộ skills: create-standalone-module.
làm nhiệm vụ gen code khi có yêu cầu tạo mới một module.
Mong muốn của tôi như sau:
Khi có yêu cầu thì:

Step 1: Tạo menu
=> phân tích và tạo menu trong const NEW_MENUS trong file mew-menu.config.ts.

Step 2: Tạo path router trong app-router.module và app-router.minimal
app-router.module => bắt buộc để router đến module
app-router.minimal => optional - đây là router dành cho chế độ phát triển với một số module cụ thể để giảm tải cho máy yếu => hỏi trước khi implement

Step 3: Tạo module theo skills => đã có skills

hiện tại bước 3 đã có skills rồi nhưng bước 1 và 2 thì chưa có

Tôi dự định tôi sẽ viết code demo cho step 1 và step 2 và kiro sẽ phân tích dựa trên full code cho skills này để gen ra bộ skill chuẩn cho tôi.

```
Tôi vừa tự implement một module demo hoàn chỉnh để làm canonical implementation cho skill `create-standalone-module`.

Hãy phân tích implementation demo và nâng cấp skill hiện có để hỗ trợ đầy đủ workflow tạo standalone module.

## Source of truth

Chỉ sử dụng các nguồn sau:

1. Code demo tôi vừa implement.
2. Git diff của implementation demo.
3. Full content của các file trực tiếp bị thay đổi.
4. Steering hiện có trong `.kiro/steering/`.
5. Skill `create-standalone-module` hiện tại.

Không scan các module khác trong project để tự tìm implementation mẫu. Không suy luận convention từ code legacy.

Trước tiên, hãy xác định chính xác:

* File menu config thực tế.
* Constant `NEW_MENUS`.
* Type/interface của menu.
* Main router file thực tế.
* Minimal router file thực tế.
* Skill `create-standalone-module` hiện tại.
* Module demo vừa được tạo.
* Mối liên hệ giữa menu path, router path và module path.

Không giả định filename. Phải sử dụng đúng tên và đường dẫn tìm được trong project.

## Workflow mới của skill

### Step 0 — Analyze and confirm

1. Phân tích yêu cầu tạo module.
2. Xác định:

   * Module name.
   * Module location.
   * Router path.
   * Menu configuration.
   * Các thông tin bắt buộc khác được thể hiện trong code demo.
3. Kiểm tra path hoặc menu đã tồn tại hay chưa.
4. Hỏi user có muốn đăng ký module vào minimal router hay không.
5. Đây là câu hỏi bắt buộc trước khi implement nếu user chưa cung cấp lựa chọn.
6. Chỉ hỏi thêm những thông tin quan trọng không thể suy ra an toàn từ yêu cầu.

### Step 1 — Create menu

1. Cập nhật constant `NEW_MENUS` trong đúng menu config file.
2. Thực hiện theo đúng structure và convention của code demo.
3. Chỉ thêm menu cần thiết, không sửa hoặc sắp xếp lại menu hiện hữu.
4. Không tự đoán label, permission, icon, parent hoặc các giá trị nghiệp vụ quan trọng nếu chưa đủ thông tin.
5. Menu router path phải thống nhất với route được tạo ở Step 2.

### Step 2 — Register routes

#### Main router

1. Luôn đăng ký route vào main router.
2. Route phải trỏ đúng tới module được tạo.
3. Thực hiện đúng lazy-loading pattern trong code demo.
4. Không sửa route không liên quan.

#### Minimal router

1. Chỉ đăng ký route vào minimal router nếu user đã đồng ý tại Step 0.
2. Không mặc định thêm hoặc bỏ qua minimal router.
3. Route trong minimal router phải thống nhất với main router.
4. Không sửa các route khác trong minimal router.

### Step 3 — Create standalone module

1. Tái sử dụng toàn bộ hướng dẫn tạo module đang có trong skill hiện tại.
2. Không làm mất hoặc đơn giản hóa các convention đã tồn tại.
3. Không duplicate hướng dẫn nếu có thể tách thành reference và tái sử dụng.
4. Module được tạo phải khớp với route đã đăng ký.

## Consistency rules

Sau khi hoàn tất, phải xác nhận:

* Menu path khớp main router path.
* Minimal router path khớp main router path nếu được tạo.
* Router trỏ đúng module.
* Tên, đường dẫn và import nhất quán.
* Không có menu hoặc route trùng lặp.
* Không có thay đổi ngoài phạm vi.

## Cấu trúc skill

Giữ nguyên identity và vị trí của skill hiện tại.

Nếu `SKILL.md` trở nên quá dài, hãy tổ chức lại theo progressive disclosure:

* `SKILL.md`: activation description, input, workflow và quality checks.
* `references/menu-registration.md`: convention từ menu demo.
* `references/router-registration.md`: convention từ router demo.
* Reference tạo module hiện tại: giữ nguyên hoặc tổ chức lại mà không làm mất nội dung.

Không tạo thêm custom agent nếu chưa cần thiết.

## Yêu cầu thực hiện

1. Trước tiên, phân tích code demo và git diff.
2. Trình bày ngắn gọn pattern đã rút ra cho Step 1 và Step 2.
3. Chỉ ra các điểm còn mơ hồ, nếu có.
4. Sau đó nâng cấp skill hiện tại.
5. Không chỉnh sửa production source trong quá trình nâng cấp skill.
6. Không sửa steering hiện có.
7. Không tự thêm convention không xuất hiện trong steering hoặc code demo.
8. Kiểm tra skill sau khi cập nhật để bảo đảm:

   * Description kích hoạt đúng khi user yêu cầu tạo module mới.
   * Minimal router luôn được hỏi trước khi implement.
   * Các reference chỉ được đọc khi cần.
   * Workflow cũ của Step 3 vẫn được bảo toàn.

Sau khi hoàn thành, hãy cung cấp:

1. Danh sách file skill đã tạo hoặc chỉnh sửa.
2. Pattern Step 1 và Step 2 được trích xuất từ demo.
3. Nội dung workflow mới.
4. Những phần của skill cũ được giữ nguyên.
5. Một prompt test cho trường hợp thêm minimal router.
6. Một prompt test cho trường hợp không thêm minimal router.
7. Một checklist để xác nhận menu, route và module đồng bộ.
```
