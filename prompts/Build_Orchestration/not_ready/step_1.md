Final validation report trả về `NOT_READY`.

Hãy phân tích các blocker khiến steering chưa thể được công nhận là source of truth và tạo một remediation batch nhỏ, có phạm vi chính xác.

Đây là bước phân tích và lập kế hoạch. Chưa được chỉnh sửa bất kỳ file nào.

## Nguồn phân tích

Đọc:

1. Final validation report.
2. Kết quả behavioral validation.
3. Decision register.
4. Remediation plan.
5. Kết quả và targeted re-audit của Batch 1–4.
6. Trạng thái hiện tại của `.kiro/steering/angular-skills/`.

Không thực hiện lại toàn bộ audit nếu final report đã có đủ bằng chứng.

## Phase 1 — Extract readiness blockers

Chỉ lấy các vấn đề trực tiếp làm kết quả trở thành `NOT_READY`.

Tạo bảng:

| Blocker ID | Nguồn phát hiện | Loại | Severity | Acceptance criterion bị fail | Bằng chứng |
| ---------- | --------------- | ---- | -------- | ---------------------------- | ---------- |

Phân loại blocker:

- `KNOWLEDGE_GAP`: Rule bắt buộc còn thiếu hoặc partial.
- `CONFLICT`: Rule vẫn xung đột.
- `DECISION_MISMATCH`: Steering chưa tuân thủ decision register.
- `ANGULAR_16_INCOMPATIBLE`: API hoặc pattern không tương thích.
- `ACTIVATION_GAP`: Steering không được kích hoạt đúng.
- `SEMANTIC_LOSS`: Rule bị mất ý nghĩa sau tối ưu.
- `BEHAVIORAL_FALSE_NEGATIVE`: Không phát hiện violation được cài vào.
- `BEHAVIORAL_FALSE_POSITIVE`: Báo sai code hợp lệ.
- `VALIDATION_DESIGN_ISSUE`: Test scenario hoặc expected result có vấn đề.
- `OTHER`.

## Phase 2 — Validate each blocker

Với mỗi blocker:

1. Đối chiếu với steering hiện tại.
2. Đối chiếu với decision register.
3. Kiểm tra bằng chứng trong final report.
4. Xác định đây là:
   - `CONFIRMED_BLOCKER`
   - `FALSE_ALARM`
   - `NEEDS_HUMAN_DECISION`
   - `VALIDATION_ISSUE`

5. Không mặc định mọi failed scenario đều do steering sai.
6. Nếu behavioral test sai do fixture hoặc expected result không đúng, không sửa steering để làm test pass.
7. Không dùng production source để suy luận convention.

Tạo bảng:

| Blocker ID | Kết luận | Root cause | Steering cần sửa? | Validation cần sửa? | Human decision? |
| ---------- | -------- | ---------- | ----------------: | ------------------: | --------------: |

## Phase 3 — Determine minimal remediation

Chỉ đưa vào remediation batch các `CONFIRMED_BLOCKER`.

Với mỗi blocker, cung cấp:

- File steering liên quan.
- Rule hiện tại.
- Rule cuối cùng được decision register yêu cầu.
- Thay đổi tối thiểu đề xuất.
- Rủi ro.
- Cách kiểm tra sau khi sửa.

Nếu blocker cần user quyết định, không tự đưa ra final rule.

## Phase 4 — Separate steering fixes from validation fixes

Chia thành hai nhóm độc lập:

### A. Steering remediation

Chỉ gồm lỗi thực sự nằm trong steering:

- Missing/partial rule.
- Conflict.
- Decision mismatch.
- Angular 16 incompatibility.
- Activation gap.
- Semantic loss.

### B. Validation remediation

Chỉ gồm lỗi của quy trình kiểm chứng:

- Fixture không đại diện đúng rule.
- Expected result sai.
- Pattern matching test sai.
- Scenario mơ hồ.
- Behavioral test nạp sai steering.
- Tiêu chí `READY` được áp dụng sai.

Không sửa steering để bù cho lỗi validation.

## Remediation batch proposal

Tạo một micro-batch với:

| Fix ID | Blocker ID | Target file | Thay đổi đề xuất | Validation |
| ------ | ---------- | ----------- | ---------------- | ---------- |

Yêu cầu:

1. Chỉ sửa file trực tiếp liên quan đến blocker.
2. Không xử lý P2/P3 không ảnh hưởng readiness.
3. Không tiếp tục tối ưu token.
4. Không tổ chức lại folder.
5. Không sửa source, agent, skill hoặc prompt review.
6. Không archive `.amazonq`.
7. Không mở rộng convention.
8. Không làm lại Batch 1–4.

## Kết quả đầu ra

Cung cấp:

1. Lý do tổng thể dẫn tới `NOT_READY`.
2. Danh sách confirmed blocker.
3. Danh sách false alarm.
4. Danh sách validation issue.
5. Human decision còn thiếu.
6. Micro-batch đề xuất.
7. Acceptance criteria cần chạy lại sau micro-batch.
8. Chờ tôi xác nhận; chưa chỉnh sửa file.
