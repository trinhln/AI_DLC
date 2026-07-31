Phân tích `NOT_READY` đã hoàn thành và micro-batch remediation đã được user xác nhận.

Hãy thực hiện đúng micro-batch đã duyệt, sau đó chạy targeted readiness validation.

## Điều kiện bắt đầu

Trước khi chỉnh sửa, xác nhận:

1. Mọi `HUMAN_DECISION` liên quan đã được user quyết định.
2. Chỉ có `CONFIRMED_BLOCKER` và `VALIDATION_ISSUE` đã được phê duyệt.
3. Working tree không có thay đổi ngoài phạm vi đã biết.
4. Danh sách file sửa khớp micro-batch.
5. Không có blocker mới chưa được phân tích.

Nếu chưa đáp ứng, dừng và báo lại.

## Phase 1 — Confirm scope

Tạo bảng:

| Fix ID | Blocker ID | Loại | Target file | Thay đổi đã duyệt | Validation cần chạy lại |
| ------ | ---------- | ---- | ----------- | ----------------- | ----------------------- |

Loại fix chỉ được là:

- `STEERING_FIX`
- `VALIDATION_FIX`

Không gộp hai loại vào cùng một thay đổi nếu có thể tách riêng.

## Phase 2 — Apply steering fixes

Chỉ áp dụng cho `STEERING_FIX` đã được duyệt.

Yêu cầu:

1. Chỉnh sửa tối thiểu.
2. Chỉ xử lý đúng blocker liên quan.
3. Tuân thủ decision register.
4. Không mở rộng convention.
5. Không thay đổi rule khác.
6. Không tổ chức lại folder.
7. Không tiếp tục tối ưu token.
8. Không thay đổi front matter nếu blocker không liên quan activation.
9. Không dùng source code để suy luận convention.
10. Không sửa `.amazonq/rules/coding_conventions/`.

## Phase 3 — Apply validation fixes

Chỉ áp dụng cho `VALIDATION_FIX` đã được duyệt.

Yêu cầu:

1. Chỉ sửa fixture, expected result hoặc validation logic bị xác định là sai.
2. Không hạ thấp acceptance criteria để đạt `READY`.
3. Không bỏ seeded violation hợp lệ.
4. Không sửa steering để phù hợp một fixture sai.
5. Không thay đổi rule hoặc decision register.
6. Giữ lại khả năng phát hiện false positive và false negative.

## Không được thay đổi

- Production source.
- Agent.
- Skill.
- Prompt review.
- Build/test configuration của Angular.
- Knowledge-base archive.
- Artifact không nằm trong micro-batch.
- Các Batch 1–4 đã được xác nhận, ngoài đúng blocker cần sửa.

## Phase 4 — Diff validation

Sau chỉnh sửa:

1. Kiểm tra mọi thay đổi map được tới Fix ID.
2. Kiểm tra không có file ngoài scope.
3. Kiểm tra không có semantic change ngoài decision.
4. Kiểm tra steering fix không tạo:
   - Conflict mới.
   - Duplicate mới.
   - Angular 16 incompatibility mới.
   - Activation gap mới.
   - Strength hoặc scope mismatch mới.

5. Kiểm tra validation fix không làm yếu acceptance criteria.

## Phase 5 — Targeted readiness validation

Chỉ chạy lại:

1. Acceptance criteria đã fail trong final validation trước.
2. Behavioral scenario liên quan trực tiếp.
3. Regression checks tối thiểu cho rule bị chỉnh sửa.

Với mỗi blocker:

| Blocker ID | Trạng thái trước | Fix đã áp dụng | Validation chạy lại | Kết quả |
| ---------- | ---------------- | -------------- | ------------------- | ------- |

Nếu blocker liên quan behavioral validation, chạy lại:

- Scenario từng fail.
- Ít nhất một scenario hợp lệ để kiểm tra false positive.
- Ít nhất một scenario violation khác cùng category để kiểm tra regression.

Không chạy lại toàn bộ Batch 1–4 nếu không có bằng chứng cần thiết.

## Phase 6 — Readiness decision

Chỉ trả về `READY` khi:

1. Tất cả confirmed blocker đã được resolve.
2. Tất cả acceptance criteria từng fail hiện đã pass.
3. Không phát sinh P0/P1 mới.
4. Không còn conflict hoặc incompatibility chưa xử lý.
5. Decision register vẫn được tuân thủ.
6. Targeted behavioral validation pass.
7. Regression checks pass.

Nếu còn bất kỳ blocker nào, trả về `NOT_READY` và chỉ rõ blocker còn lại.

## Kết quả đầu ra

Cung cấp:

1. Danh sách Fix ID đã thực hiện.
2. Danh sách file đã thay đổi.
3. Diff đầy đủ để user review.
4. Targeted validation results.
5. Regression results.
6. Blocker đã resolved.
7. Blocker còn lại.
8. Kết luận `READY` hoặc `NOT_READY`.
9. Xác nhận:
   - Không sửa source.
   - Không sửa agent/skill/prompt.
   - Không mở rộng phạm vi.
   - Không làm yếu acceptance criteria.

10. Chờ user xác nhận; không tự thực hiện knowledge-base cutover.
