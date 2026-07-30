Hãy đọc:

1. Báo cáo audit.
2. Remediation plan.
3. Decision register đã được tôi xác nhận.

Bây giờ thực hiện `Batch 1 — Critical compatibility and conflicts`.

## Phạm vi

Chỉ xử lý:

- Các finding `P0` đã được quyết định.
- Angular API hoặc pattern không tương thích Angular 16.
- Conflict giữa project coding convention và steering.
- Rule có thể khiến Kiro sinh code sai kiến trúc hoặc làm build fail.

Không xử lý P2, P3, duplicate, wording hoặc tối ưu token trong batch này.

## Source of truth

Áp dụng thứ tự:

1. Decision register đã được tôi xác nhận.
2. Coding convention đặc thù của project.
3. Constraint Angular 16 và dependency thực tế.
4. Angular official recommendation tương thích.

Không dùng source code hiện hữu để suy luận convention.

## Trước khi chỉnh sửa

1. Xác nhận working tree hiện tại.
2. Liệt kê chính xác:
   - Finding ID được xử lý.
   - File steering sẽ chỉnh sửa.
   - Nội dung dự kiến thay đổi.

3. Kiểm tra mọi finding đều có decision đã được xác nhận.
4. Nếu finding chưa có quyết định rõ ràng, bỏ qua và báo lại.
5. Không sửa file ngoài danh sách Batch 1.

## Thực hiện

1. Chỉnh sửa tối thiểu các file trong `.kiro/steering/angular-skills/`.
2. Bảo toàn những rule không liên quan.
3. Không sửa:
   - `.amazonq/rules/coding_conventions/`
   - Source code.
   - Agent.
   - Skill.
   - Prompt review.
   - Build configuration.

4. Không tự thêm convention mới.
5. Không thay đổi mức độ `MUST`, `MUST NOT`, `SHOULD` ngoài decision register.
6. Không gộp hoặc tổ chức lại file nếu Batch 1 không yêu cầu.
7. Bảo đảm nội dung cuối cùng chỉ sử dụng API và pattern tương thích Angular 16.

## Validation sau chỉnh sửa

Sau khi cập nhật:

1. Kiểm tra diff chỉ chứa các file đã được phê duyệt.
2. Đối chiếu từng thay đổi với Finding ID và decision tương ứng.
3. Kiểm tra lại:
   - Không còn conflict P0 đã xử lý.
   - Không còn Angular 16 incompatibility thuộc Batch 1.
   - Không làm mất project-specific convention.
   - Không tạo duplicate hoặc conflict mới.
   - Front matter vẫn hợp lệ.

4. Thực hiện targeted re-audit chỉ cho các finding thuộc Batch 1.
5. Không chạy unit test hoặc build vì batch này chỉ thay đổi tài liệu steering.

## Kết quả đầu ra

Cung cấp:

1. Danh sách Finding ID đã xử lý.
2. Danh sách file đã thay đổi.
3. Tóm tắt từng thay đổi.
4. Diff để tôi review.
5. Kết quả targeted re-audit:

| Finding ID | Trạng thái trước | Decision | Trạng thái sau | Kết quả |
| ---------- | ---------------- | -------- | -------------- | ------- |

6. Finding nào bị bỏ qua và lý do.
7. Xác nhận không có file ngoài phạm vi bị thay đổi.
8. Chờ tôi review và xác nhận; không tự chuyển sang Batch 2.
