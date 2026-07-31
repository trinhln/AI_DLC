Hãy phân tích orchestration prompt review hiện tại tại:

`.kiro/prompts/review-pr.md`

## Mục tiêu

Chuẩn bị chuyển nguồn coding convention của workflow review:

- Nguồn cũ: `.amazonq/rules/coding_conventions`
- Source of truth mới: `.kiro/steering/angular-skills`

Workflow review hiện tại đã tồn tại và đang được sử dụng. Không thiết kế lại workflow từ đầu.

## Yêu cầu phân tích

### 1. Phân tích orchestration hiện tại

Xác định:

- Cách workflow lấy và giới hạn diff.
- Các phase review hiện có.
- Các custom agent được gọi.
- Trách nhiệm của từng agent.
- Cách tổng hợp kết quả.
- Cách tính điểm trên thang điểm 100.
- Điều kiện pass/fail với ngưỡng 85 điểm.
- Cách yêu cầu cập nhật khi kết quả dưới 85.
- Các validation hoặc quality gate đang có.

### 2. Tìm dependency vào convention cũ

Tìm tất cả dependency trực tiếp và gián tiếp từ workflow review đến:

- `.amazonq/rules/coding_conventions`
- Các file convention cũ khác.
- Nội dung convention được duplicate trong prompt.
- Agent con hoặc prompt phụ vẫn đọc convention cũ.
- Script, command hoặc reference gián tiếp được workflow sử dụng.

Với mỗi dependency, báo cáo:

- File.
- Section.
- Cách dependency được sử dụng.
- Có phải dependency runtime hay chỉ là tài liệu tham khảo.
- Hành động migration cần thiết.

### 3. Lập mapping sang Angular steering

Dựa trên loại file có thể xuất hiện trong diff, xác định steering tương ứng trong:

`.kiro/steering/angular-skills`

Ví dụ các nhóm cần xem xét:

- Component TypeScript.
- Template HTML.
- Styles.
- Form.
- Service và API.
- Model, interface, enum và constant.
- Routing.
- RxJS và subscription handling.
- Unit test.
- Shared utility hoặc loại file khác.

Không load toàn bộ `angular-skills` mặc định.

Mapping phải dựa trên:

- Loại file trong diff.
- Đường dẫn file.
- `inclusion`.
- `fileMatchPattern`.
- Nội dung thay đổi khi chỉ extension chưa đủ để phân loại.

### 4. Xác định cơ chế load convention khi review

Đề xuất cách để `review-pr.md`:

1. Đọc danh sách file thay đổi trước.
2. Phân loại từng file.
3. Xác định steering phù hợp.
4. Chỉ load steering cần thiết.
5. Truyền hoặc yêu cầu các review agent sử dụng cùng convention đã được chọn.
6. Ưu tiên core steering và các rule `MUST`/`MUST NOT`.
7. Không suy luận convention từ source code application.
8. Không sử dụng code legacy làm canonical convention.
9. Không duplicate toàn bộ convention vào orchestration prompt.

### 5. Kiểm tra ảnh hưởng đến scoring

Đánh giá việc thay source of truth có ảnh hưởng đến:

- Rubric chấm điểm.
- Trọng số từng category.
- Severity của finding.
- Quy tắc trừ điểm.
- Ngưỡng pass `>= 85`.
- Ngưỡng fail `< 85`.
- Cơ chế yêu cầu developer cập nhật code.

Không tự thay đổi scoring trong bước phân tích này.

Nếu steering mới thiếu dữ liệu cần thiết cho một tiêu chí chấm điểm hiện tại, phải ghi nhận thành migration blocker, không tự lấy lại rule cũ để bù vào.

## Ràng buộc

- Chỉ phân tích, không chỉnh sửa file.
- Không tạo prompt, skill hoặc agent mới.
- Không sửa `.kiro/steering/angular-skills`.
- Không thay đổi scoring.
- Không scan source code để suy luận convention.
- Không khôi phục convention cũ đã được loại bỏ.
- `.kiro/steering/angular-skills` là source of truth duy nhất sau migration.

## Kết quả đầu ra

### Current workflow

- Các phase.
- Agents tham gia.
- Scoring và quality gate.

### Legacy dependencies

| Dependency | File/section | Usage | Runtime/reference | Migration action |
| ---------- | ------------ | ----- | ----------------- | ---------------- |

### Steering mapping

| Changed file/task type | Steering cần load | Activation basis | Consumer agent |
| ---------------------- | ----------------- | ---------------- | -------------- |

### Required changes

Liệt kê thay đổi tối thiểu cần thực hiện cho:

- `.kiro/prompts/review-pr.md`
- Các agent được orchestration gọi.
- Các prompt hoặc file phụ liên quan.

### Scoring impact

- Nội dung được giữ nguyên.
- Nội dung cần mapping lại.
- Blocker nếu có.

### Migration readiness

Chỉ trả về:

- `READY_TO_MIGRATE`
- `BLOCKED`

Nếu `BLOCKED`, liệt kê rõ blocker và quyết định cần tôi xác nhận.

Không implement migration trong bước này.
