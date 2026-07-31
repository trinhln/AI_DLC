Knowledge-base cutover đã được user xác nhận:

- `.kiro/steering/angular-skills/` là source of truth duy nhất.
- Không còn active runtime dependency vào conventions Amazon Q.
- Prompt review Amazon Q cũ đã được archive và bảo toàn.

Bây giờ hãy phân tích prompt review Amazon Q cũ và tạo một `Review Workflow Specification` dành cho Kiro.

Đây là bước phân tích chỉ đọc. Không sửa prompt cũ, steering, agent, skill hoặc source code.

## Nguồn phân tích

Đọc:

1. Prompt review Amazon Q đã archive.
2. Các script hoặc artifact trực tiếp được prompt đó sử dụng.
3. Các custom review agent Kiro hiện có, chỉ để đánh giá khả năng tái sử dụng.
4. `.kiro/steering/angular-skills/` để xác nhận knowledge source mới.
5. Final validation report và decision register nếu cần xác minh source of truth.

Không scan production source để suy luận review convention.

## Mục tiêu

Trích xuất từ prompt cũ:

1. Cách xác định review scope.
2. Cách lấy diff.
3. Các phase hoặc category review.
4. Severity model.
5. Rubric chấm điểm 100.
6. Ngưỡng pass/fail 85.
7. Cách khấu trừ điểm.
8. Cách xử lý duplicate finding.
9. Evidence requirements.
10. Output format.
11. Điều kiện dừng hoặc hỏi lại user.
12. Tooling và dependency đặc thù của Amazon Q.
13. Những phần cần thay đổi để phù hợp Kiro.

## Phase 1 — Decompose legacy prompt

Phân loại từng phần của prompt Amazon Q:

| Legacy section | Mục đích | Phân loại | Hành động đề xuất |
| -------------- | -------- | --------- | ----------------- |

Dùng các classification:

- `WORKFLOW`: Quy trình review có thể giữ.
- `SCORING`: Rubric hoặc threshold.
- `OUTPUT_FORMAT`: Cấu trúc báo cáo.
- `KNOWLEDGE_DUPLICATE`: Coding convention đã có trong steering.
- `AMAZON_Q_SPECIFIC`: Tool, path hoặc command riêng Amazon Q.
- `PROJECT_SPECIFIC`: Quy trình đặc thù NBE cần bảo toàn.
- `OBSOLETE`: Không còn phù hợp.
- `AMBIGUOUS`: Cần user quyết định.

Không copy `KNOWLEDGE_DUPLICATE` sang specification. Thay bằng reference tới steering.

## Phase 2 — Analyze review scope

Xác định prompt cũ hỗ trợ những mode nào:

- Review uncommitted local changes.
- Review staged changes.
- Review branch so với base branch.
- Review PR diff.
- Review file được user chỉ định.
- Mode khác.

Với mỗi mode, mô tả:

- Input bắt buộc.
- Cách lấy diff.
- Base/reference.
- File được include.
- File cần exclude.
- Hành vi khi diff trống.
- Hành vi khi diff quá lớn.
- Hành vi với generated file, lock file hoặc binary.

Không tự thêm mode nếu prompt cũ và workflow hiện tại không yêu cầu.

## Phase 3 — Review categories

Trích xuất các category review hiện có, ví dụ:

- Coding convention.
- Angular architecture.
- Correctness.
- Performance.
- Security.
- RxJS.
- Maintainability.
- Testing.
- Scope control.

Với mỗi category:

| Category | Nguồn knowledge | Review responsibility | Có agent hiện tại hỗ trợ? |
| -------- | --------------- | --------------------- | ------------------------: |

Coding convention phải sử dụng steering, không chứa lại rule trong workflow specification.

Nếu custom review agent hiện tại có thể tái sử dụng, ghi rõ:

- Agent name.
- Trách nhiệm.
- Input cần nhận.
- Output cần trả lại.
- Có overlap với agent khác không.

Không sửa hoặc gọi các agent này trong bước phân tích.

## Phase 4 — Severity and evidence

Trích xuất severity model hiện có.

Với mỗi severity, làm rõ:

- Định nghĩa.
- Điều kiện sử dụng.
- Ảnh hưởng tới điểm.
- Có blocking hay không.

Mỗi finding trong workflow mới phải có:

1. File path.
2. Line hoặc vùng code liên quan nếu xác định được.
3. Mô tả vấn đề.
4. Evidence từ diff.
5. Steering rule hoặc review criterion bị vi phạm.
6. Severity.
7. Ảnh hưởng.
8. Đề xuất sửa ngắn gọn.

Không cho phép finding chỉ dựa trên preference của model.

## Phase 5 — Scoring audit

Phân tích rubric 100 điểm hiện tại:

1. Điểm ban đầu.
2. Các category và trọng số.
3. Điểm trừ theo severity.
4. Maximum deduction.
5. Blocking violation.
6. Cách xử lý nhiều finding cùng root cause.
7. Cách làm tròn điểm.
8. Điều kiện pass: `score >= 85`.
9. Điều kiện fail: `score < 85`.

Phát hiện:

- Điểm trừ bị trùng.
- Tổng trọng số không bằng 100.
- Một lỗi có thể bị trừ ở nhiều category.
- Severity và deduction không tương xứng.
- Code không có finding nhưng vẫn không đạt 100.
- Blocking issue nhưng điểm vẫn pass.
- Rubric không deterministic.

Không tự thay đổi rubric. Đánh dấu nội dung cần user quyết định.

## Phase 6 — Amazon Q to Kiro mapping

Tạo bảng:

| Amazon Q dependency | Vai trò cũ | Kiro equivalent | Có thể migrate | Ghi chú |
| ------------------- | ---------- | --------------- | -------------: | ------- |

Bao gồm:

- Tool names.
- Prompt invocation.
- Context/rule loading.
- Diff file.
- Shell command.
- Agent/sub-agent.
- File path.
- Output artifact.

Không giả định tool name Kiro. Kiểm tra schema và agent hiện có trong project.

## Phase 7 — Design Review Workflow Specification

Tạo specification gồm:

### 1. Purpose

Mục tiêu và phạm vi review.

### 2. Supported review modes

Input và cách xác định diff cho từng mode.

### 3. Knowledge-source policy

- Steering là source of truth.
- Chỉ nạp steering phù hợp với file trong diff.
- Không dùng `.amazonq/rules`.
- Không suy luận convention từ code legacy.
- Không duplicate convention trong agent prompt.

### 4. Review pipeline

Quy trình dự kiến:

```text
Validate request
→ Resolve review mode
→ Collect diff
→ Classify changed files
→ Load applicable steering
→ Execute review categories
→ Normalize and deduplicate findings
→ Calculate score
→ Produce report
```

Chỉ giữ phase có căn cứ từ workflow cũ hoặc requirement đã xác nhận.

### 5. Finding contract

Schema bắt buộc của một finding.

### 6. Severity model

Định nghĩa severity đã trích xuất.

### 7. Scoring contract

Rubric, deduction, threshold và blocking condition.

### 8. Output contract

Format báo cáo cuối cùng.

### 9. Failure handling

Xử lý:

- Không có diff.
- Không xác định được base branch.
- Steering cần thiết không được tìm thấy.
- Command thất bại.
- Diff quá lớn.
- Agent phụ trợ thất bại.
- Không đủ bằng chứng để kết luận.

### 10. Read-only policy

Review workflow:

- Không sửa source code.
- Không tự chạy implementation.
- Không tự commit.
- Không tự tạo test.
- Không thay đổi steering.
- Chỉ đưa finding và recommendation.

## Human decisions

Liệt kê riêng những điểm cần user xác nhận:

| Decision ID | Chủ đề | Legacy behavior | Phương án | Đề xuất |
| ----------- | ------ | --------------- | --------- | ------- |

Chỉ hỏi những quyết định thực sự ảnh hưởng hành vi review, scoring hoặc chi phí token.

## Kết quả đầu ra

Tạo một Review Workflow Specification riêng và cung cấp:

1. Đường dẫn specification.
2. Legacy prompt decomposition matrix.
3. Amazon Q → Kiro mapping.
4. Review modes đã xác định.
5. Review categories.
6. Scoring audit.
7. Danh sách custom review agent có thể tái sử dụng.
8. Những nội dung legacy bị loại bỏ.
9. Human decisions cần tôi xác nhận.
10. Chờ tôi xác nhận; chưa tạo hoặc chỉnh sửa `review-code` agent.
