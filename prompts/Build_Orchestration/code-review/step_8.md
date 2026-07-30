Hãy đọc:

1. Báo cáo audit ban đầu.
2. Remediation plan.
3. Decision register.
4. Kết quả và diff đã được user xác nhận của Batch 1–4.
5. Toàn bộ `.kiro/steering/angular-skills/` hiện tại.
6. `.amazonq/rules/coding_conventions/` để đối chiếu lần cuối.

Bây giờ thực hiện `Batch 5 — Full re-audit and behavioral validation`.

Đây là bước kiểm chứng chỉ đọc. Không được chỉnh sửa steering, source code, agent, skill hoặc prompt.

## Mục tiêu

Xác nhận rằng `.kiro/steering/angular-skills/`:

1. Bao phủ đầy đủ coding convention của project.
2. Không còn conflict đã biết.
3. Không còn rule không tương thích Angular 16.
4. Giữ đúng mức độ bắt buộc.
5. Có activation pattern chính xác.
6. Không còn duplicate đáng kể.
7. Không bị mất semantic sau khi tối ưu token.
8. Có thể trở thành source of truth duy nhất cho implement và code review.

## Phase 1 — Full rule re-audit

Thực hiện lại audit từ đầu, không tái sử dụng kết luận cũ mà không kiểm tra file hiện tại.

Phân loại từng atomic rule:

- `EXACT_MATCH`
- `SEMANTIC_MATCH`
- `PARTIAL`
- `MISSING_IN_STEERING`
- `CONFLICT`
- `STRENGTH_MISMATCH`
- `SCOPE_MISMATCH`
- `STEERING_ENHANCEMENT`
- `INCOMPATIBLE`
- `DUPLICATE`
- `NEEDS_HUMAN_DECISION`

Mỗi kết luận phải dẫn tới file cụ thể.

## Phase 2 — Decision compliance

Đối chiếu steering với decision register:

| Finding ID | Decision đã duyệt | Steering hiện tại | Tuân thủ | Ghi chú |
| ---------- | ----------------- | ----------------- | -------: | ------- |

Phát hiện:

- Decision chưa được áp dụng.
- Decision chỉ được áp dụng một phần.
- Nội dung steering đi xa hơn decision.
- Decision bị thay đổi trong Batch 4.
- Rule đã bị giảm hoặc tăng mức độ bắt buộc.

## Phase 3 — Angular 16 compatibility

Kiểm tra toàn bộ API và pattern Angular được đề cập trong steering.

Phân loại:

- Tương thích Angular 16.
- Tương thích nhưng cần điều kiện.
- Không tương thích.
- Không đủ bằng chứng.

Không dùng code legacy làm căn cứ xác định convention.

## Phase 4 — Activation validation

Kiểm tra:

- YAML front matter.
- `inclusion`.
- `fileMatchPattern`.
- Representative path matching.
- Overlap giữa các steering.
- Activation gap.
- Steering bị nạp quá rộng.

Tạo matrix:

| Loại task/file | Steering dự kiến được nạp | Steering thực tế match | Kết quả |
| -------------- | ------------------------- | ---------------------- | ------- |

Bao gồm:

- Component TypeScript.
- Template.
- Style.
- Service/API.
- Model/type.
- Routing.
- Form.
- RxJS logic.
- Unit test.
- File mới chưa tồn tại.

Đối với file mới, xác nhận implementation agent phải chủ động chọn steering dựa trên đường dẫn dự kiến; không yêu cầu chuyển toàn bộ steering thành `always`.

## Phase 5 — Semantic preservation

So sánh knowledge coverage trước và sau Batch 4:

1. Mỗi rule cũ còn owner rõ ràng không.
2. Điều kiện và ngoại lệ còn đầy đủ không.
3. Project-specific utilities và abstractions còn được nhắc rõ không.
4. Rule có bị rút gọn thành câu quá chung chung không.
5. Có rule nào chỉ còn reference nhưng reference không được kích hoạt không.

## Phase 6 — Behavioral validation

Tạo các tình huống kiểm tra tổng hợp nhưng không sửa production source.

### A. Convention selection

Tạo tối thiểu một scenario cho mỗi nhóm:

- Component.
- Template.
- Service/API.
- Form.
- Routing.
- Model.
- RxJS.
- Styling.

Với mỗi scenario, xác định:

- Đường dẫn file giả định.
- Steering cần nạp.
- Rule bắt buộc cần áp dụng.
- Steering không liên quan không nên nạp.

### B. Violation detection

Tạo các đoạn code nhỏ, độc lập, cố tình chứa violation đã biết.

Yêu cầu hệ thống xác định:

- Rule bị vi phạm.
- Steering file chứa rule.
- Severity.
- Cách sửa đúng.

Phải bao gồm:

- Violation `MUST NOT`.
- Violation `MUST`.
- Angular 16 incompatibility.
- Project-specific convention.
- Một đoạn code hợp lệ để kiểm tra false positive.

Không dùng source code production làm fixture.

### C. Implementation guidance

Tạo yêu cầu implement giả lập cho:

- Tạo file mới.
- Fine-tune file hiện hữu.
- Sửa bug phạm vi nhỏ.

Kiểm tra hệ thống có:

1. Chọn đúng steering.
2. Không scan project để suy luận convention.
3. Không sử dụng API sau Angular 16.
4. Không tự refactor ngoài phạm vi.
5. Không tự tạo unit test nếu không được yêu cầu.

Không thực sự tạo hoặc chỉnh sửa source file.

## Acceptance criteria

Chỉ đề xuất công nhận steering là source of truth khi:

- Không còn `P0`.
- Không còn `P1`.
- Không còn `CONFLICT` chưa quyết định.
- Không còn `INCOMPATIBLE` chưa xử lý.
- 100% project-specific mandatory rules được bao phủ.
- 100% decision register được áp dụng đúng.
- Không có activation gap nghiêm trọng.
- Không mất semantic sau Batch 4.
- Behavioral validation phát hiện đầy đủ các seeded `MUST`/`MUST NOT` violations.
- Scenario hợp lệ không bị báo lỗi sai nghiêm trọng.

Nếu không đạt, ghi rõ `NOT_READY` và liệt kê blocker. Không tự sửa.

## Báo cáo cuối cùng

Tạo final validation report gồm:

### 1. Executive summary

- `READY` hoặc `NOT_READY`.
- Coverage percentage.
- Số conflict còn lại.
- Số incompatibility còn lại.
- Số activation gap.
- Kết quả behavioral validation.

### 2. Final coverage matrix

| Rule ID | Chủ đề | Original convention | Steering owner | Trạng thái |
| ------- | ------ | ------------------- | -------------- | ---------- |

### 3. Decision compliance

Bảng tuân thủ decision register.

### 4. Angular 16 compatibility

Danh sách API/pattern đã kiểm tra.

### 5. Activation matrix

Kết quả matching theo loại file.

### 6. Behavioral test results

| Scenario | Expected | Actual | Pass/Fail |
| -------- | -------- | ------ | --------- |

### 7. Remaining issues

Phân loại theo P0–P3.

### 8. Final recommendation

Kết luận một trong hai:

- `READY`: Có thể dùng steering làm source of truth.
- `NOT_READY`: Chưa được dùng làm source of truth và cần remediation bổ sung.

## Giới hạn

1. Không sửa bất kỳ file nào ngoài việc tạo báo cáo.
2. Không chạy unit test hoặc Angular build.
3. Không xóa `.amazonq/rules/coding_conventions/`.
4. Không tự tạo review agent.
5. Không tự chuyển sang bước migration prompt Amazon Q.
6. Chờ user xác nhận kết quả.
