Hãy đọc:

1. Báo cáo audit ban đầu.
2. Remediation plan.
3. Decision register đã được xác nhận.
4. Kết quả targeted re-audit của Batch 1.
5. Diff hiện tại sau khi Batch 1 đã được user chấp thuận.

Bây giờ thực hiện `Batch 2 — Missing mandatory conventions`.

## Điều kiện bắt đầu

Chỉ thực hiện khi:

- Batch 1 đã được user xác nhận.
- Không còn finding P0 unresolved ảnh hưởng đến Batch 2.
- Working tree không chứa thay đổi ngoài phạm vi đã biết.

Nếu điều kiện chưa đáp ứng, dừng và báo lại.

## Phạm vi Batch 2

Chỉ xử lý các finding đã được phê duyệt thuộc các nhóm:

- `MISSING_IN_STEERING`.
- `PARTIAL`.
- `STRENGTH_MISMATCH`.
- `SCOPE_MISMATCH` có priority P1.
- Project-specific mandatory convention chưa được bảo toàn đầy đủ.

Không xử lý:

- Activation và `fileMatchPattern`, trừ khi decision register xác định nó thuộc Batch 2.
- Duplicate hoặc tối ưu token.
- Wording không ảnh hưởng ý nghĩa.
- Các enhancement mới chưa được user phê duyệt.
- P2/P3 ngoài phạm vi.
- Source code, agent, skill hoặc prompt review.

## Source of truth

Áp dụng thứ tự:

1. Decision register.
2. Coding convention đặc thù của project.
3. Constraint Angular 16.
4. Angular official recommendation tương thích.

Không dùng source code hiện hữu để suy luận convention.

## Trước khi chỉnh sửa

Tạo danh sách thay đổi dự kiến:

| Finding ID | Loại vấn đề | File steering | Rule hiện tại | Final rule đã duyệt | Thay đổi dự kiến |
| ---------- | ----------- | ------------- | ------------- | ------------------- | ---------------- |

Đối với mỗi finding, kiểm tra:

1. Đã có decision rõ ràng chưa.
2. Rule nên được thêm vào file steering nào.
3. Có tạo trùng với rule hiện hữu không.
4. Mức độ cuối cùng là:
   - `MUST`
   - `MUST NOT`
   - `SHOULD`
   - `MAY`

5. Phạm vi áp dụng và ngoại lệ đã rõ chưa.

Nếu chưa rõ, bỏ qua finding đó và báo lại; không tự quyết định.

## Thực hiện

1. Chỉnh sửa tối thiểu trong `.kiro/steering/angular-skills/`.
2. Bổ sung đầy đủ rule bị thiếu.
3. Hoàn thiện rule đang partial mà không làm thay đổi ý nghĩa đã duyệt.
4. Khôi phục đúng mức độ bắt buộc.
5. Làm rõ scope và ngoại lệ khi decision register đã quy định.
6. Dùng wording ngắn gọn, trực tiếp và có thể áp dụng khi implement lẫn review.
7. Dùng rõ `MUST`, `MUST NOT`, `SHOULD`, `MAY` khi cần thiết.
8. Không sao chép nguyên văn nội dung dài nếu có thể diễn đạt ngắn hơn mà vẫn giữ nguyên ý nghĩa.
9. Không duplicate một rule trong nhiều steering.
10. Không tổ chức lại toàn bộ folder trong batch này.

## Không được thay đổi

- `.amazonq/rules/coding_conventions/`.
- Source code.
- Agent.
- Skill.
- Prompt review.
- Build hoặc test configuration.
- Những file steering không nằm trong danh sách đã công bố trước khi sửa.

## Validation

Sau khi cập nhật:

1. Kiểm tra diff chỉ gồm các file Batch 2.
2. Map từng thay đổi về đúng Finding ID.
3. Kiểm tra:
   - Mọi missing rule trong batch đã được bổ sung.
   - Partial rule đã được bao phủ đầy đủ.
   - Mức độ bắt buộc không còn sai lệch.
   - Scope và ngoại lệ không bị thay đổi ngoài decision.
   - Không tạo conflict mới với Batch 1.
   - Không tạo duplicate mới.
   - Không thêm API hoặc pattern không tương thích Angular 16.

4. Thực hiện targeted re-audit cho riêng Batch 2.
5. Không chạy unit test hoặc build vì chỉ thay đổi steering.

## Kết quả đầu ra

Cung cấp:

1. Danh sách Finding ID đã xử lý.
2. Danh sách file đã thay đổi.
3. Tóm tắt rule được thêm hoặc hoàn thiện.
4. Diff để user review.
5. Kết quả targeted re-audit:

| Finding ID | Vấn đề trước | Final rule | Trạng thái sau | Kết quả |
| ---------- | ------------ | ---------- | -------------- | ------- |

6. Finding bị bỏ qua và lý do.
7. Conflict hoặc duplicate mới phát hiện, nếu có.
8. Xác nhận không có file ngoài phạm vi bị thay đổi.
9. Chờ user xác nhận; không tự chuyển sang Batch 3.
