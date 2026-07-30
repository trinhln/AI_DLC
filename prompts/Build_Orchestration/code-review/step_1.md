Hãy thực hiện một cuộc audit chỉ đọc để đánh giá mức độ đồng nhất giữa hai knowledge base:

- Coding conventions cũ: `.amazonq/rules/coding_conventions/`
- Angular steering hiện tại: `.kiro/steering/angular-skills/`

Bộ steering được xây dựng từ coding conventions cũ và có bổ sung kiến thức từ Angular official skills. Vì vậy, không được mặc định mọi nội dung chỉ có trong steering đều là sai.

## Mục tiêu

Xác định:

1. Coding conventions cũ đã được chuyển đầy đủ sang steering chưa.
2. Ý nghĩa và mức độ bắt buộc của từng rule có được giữ nguyên không.
3. Có rule nào bị thiếu, rút gọn quá mức hoặc thay đổi ý nghĩa không.
4. Có xung đột giữa convention của project và nội dung bổ sung từ Angular official skills không.
5. Có nội dung nào không tương thích với Angular 16 hoặc kiến trúc NBE không.
6. Cấu hình inclusion và `fileMatchPattern` có làm rule không được kích hoạt đúng lúc không.

## Giới hạn

1. Chỉ đọc và phân tích hai folder được chỉ định.
2. Có thể đọc cấu hình Angular version và dependency của project để xác nhận compatibility.
3. Không scan source code để tự suy luận coding convention.
4. Không sửa bất kỳ file rule, steering hoặc source code nào.
5. Không tự động chọn bên thắng khi phát hiện conflict.
6. Không đánh giá khác biệt câu chữ là conflict nếu hai rule có cùng ý nghĩa.
7. Không yêu cầu hai folder phải có cấu trúc file giống nhau; so sánh theo nội dung và ý nghĩa của rule.

## Phase 1 — Inventory

1. Liệt kê toàn bộ file trong hai folder.
2. Xác định chủ đề của từng file, ví dụ:
   - Architecture.
   - Component.
   - Template.
   - Service và HTTP.
   - RxJS.
   - Forms.
   - Routing.
   - Models và typing.
   - Naming.
   - Import.
   - Error handling.
   - Performance.
   - Testing.
   - Styling.

3. Chỉ ra chủ đề nào chỉ xuất hiện ở một trong hai knowledge base.

## Phase 2 — Normalize rules

Phân rã nội dung thành các atomic rule độc lập.

Mỗi rule cần có:

- Rule ID tạm thời.
- Chủ đề.
- Nội dung chuẩn hóa ngắn gọn.
- Nguồn file.
- Mức độ: `MUST`, `MUST NOT`, `SHOULD`, `MAY` hoặc chưa xác định.
- Phạm vi file/code được áp dụng.
- Điều kiện hoặc ngoại lệ nếu có.

Không gộp nhiều yêu cầu độc lập thành một rule.

## Phase 3 — Compare

Đối chiếu từng rule trong coding conventions cũ với steering và phân loại:

- `EXACT_MATCH`: Nội dung tương đương hoàn toàn.
- `SEMANTIC_MATCH`: Khác cách diễn đạt nhưng cùng yêu cầu.
- `PARTIAL`: Steering chỉ bao phủ một phần.
- `MISSING_IN_STEERING`: Rule cũ chưa có trong steering.
- `CONFLICT`: Hai nguồn đưa ra yêu cầu trái nhau.
- `STRENGTH_MISMATCH`: Mức độ bắt buộc đã thay đổi.
- `SCOPE_MISMATCH`: Nội dung giống nhau nhưng phạm vi áp dụng khác.
- `STEERING_ENHANCEMENT`: Chỉ có trong steering và là bổ sung hợp lệ.
- `INCOMPATIBLE`: Không phù hợp Angular 16 hoặc constraint của project.
- `DUPLICATE`: Bị lặp lại ở nhiều steering.

Mỗi kết luận phải dẫn rõ file và heading hoặc vị trí rule ở cả hai nguồn.

## Phase 4 — Angular 16 compatibility

Với nội dung chỉ xuất hiện trong steering:

1. Xác định đây có phải kiến thức bổ sung từ Angular official skills không.
2. Kiểm tra khả năng tương thích với Angular 16.
3. Đặc biệt chú ý các API hoặc pattern xuất hiện sau Angular 16.
4. Phân loại:
   - Hợp lệ cho NBE.
   - Hợp lệ nhưng cần điều kiện áp dụng.
   - Không tương thích Angular 16.
   - Chưa đủ thông tin để kết luận.

Không dùng source code hiện hữu làm bằng chứng cho coding convention.

## Phase 5 — Activation audit

Kiểm tra front matter của từng steering:

- `inclusion`.
- `fileMatchPattern`.
- Pattern đơn hoặc danh sách pattern.
- Phạm vi file thực tế mà rule dự kiến áp dụng.

Phát hiện:

1. Rule tồn tại nhưng pattern không match loại file cần áp dụng.
2. Pattern quá hẹp, làm bỏ sót file.
3. Pattern quá rộng, gây nạp context không cần thiết.
4. Rule liên quan nhiều loại file nhưng chỉ match một loại.
5. Core convention đáng lẽ luôn được nạp nhưng đang để `fileMatch`.
6. Front matter không hợp lệ hoặc có nguy cơ không được Kiro nhận diện.

## Báo cáo kết quả

Tạo một báo cáo Markdown riêng, không sửa hai knowledge base.

Báo cáo phải gồm:

### 1. Executive summary

- Tổng số rule trong coding conventions cũ.
- Tỷ lệ được steering bao phủ đầy đủ.
- Số rule partial.
- Số rule missing.
- Số conflict.
- Số strength/scope mismatch.
- Số steering enhancement hợp lệ.
- Số rule không tương thích Angular 16.
- Số activation gap.

### 2. Coverage matrix

| ID  | Chủ đề | Coding convention cũ | Steering tương ứng | Trạng thái | Severity | Ghi chú |
| --- | ------ | -------------------- | ------------------ | ---------- | -------- | ------- |

### 3. Missing and partial rules

Liệt kê đầy đủ các rule cần được xem xét bổ sung hoặc hoàn thiện.

### 4. Conflicts

Với mỗi conflict, trình bày:

- Rule cũ.
- Rule steering.
- Bản chất xung đột.
- Ảnh hưởng tới implement và review.
- Đề xuất lựa chọn, nhưng không tự sửa.

### 5. Steering enhancements

Phân biệt rõ:

- Bổ sung hợp lệ.
- Bổ sung cần điều kiện.
- Bổ sung không tương thích Angular 16.
- Bổ sung chưa thể xác minh.

### 6. Activation issues

Liệt kê steering file, pattern hiện tại, phạm vi mong muốn và vấn đề có thể xảy ra.

### 7. Recommended actions

Chia thành:

- `Critical`: Conflict hoặc rule bắt buộc bị thiếu.
- `High`: Strength mismatch, incompatibility hoặc activation gap quan trọng.
- `Medium`: Partial coverage hoặc scope chưa chính xác.
- `Low`: Duplicate, wording hoặc tối ưu token.

## Quality requirements

1. Mọi kết luận phải có bằng chứng từ file cụ thể.
2. Không suy đoán rule dựa trên tên file.
3. Không đánh dấu enhancement là conflict chỉ vì conventions cũ không có.
4. Không đánh giá theo số lượng file; đánh giá theo atomic rule.
5. Nếu không chắc chắn, ghi `NEEDS_HUMAN_DECISION`.
6. Không chỉnh sửa knowledge base trong bước audit này.

Sau khi hoàn thành, chỉ cung cấp:

1. Đường dẫn báo cáo.
2. Executive summary.
3. Danh sách các vấn đề Critical và High.
4. Những mục cần tôi quyết định thủ công.
