Final validation report đã kết luận:

`READY — .kiro/steering/angular-skills có thể trở thành source of truth.`

Hãy chuẩn bị kế hoạch chốt migration knowledge base từ Amazon Q sang Kiro.

Đây là bước kiểm tra và lập kế hoạch. Chưa được di chuyển hoặc xóa file.

## Mục tiêu

1. Xác nhận `.kiro/steering/angular-skills/` là source of truth duy nhất về Angular coding convention.
2. Xác định mọi dependency còn trỏ tới `.amazonq/rules/coding_conventions/`.
3. Chuẩn bị loại conventions cũ khỏi runtime context mà vẫn bảo toàn lịch sử.
4. Không để agent hoặc prompt sử dụng đồng thời hai knowledge base.

## Kiểm tra dependency

Tìm reference tới:

- `.amazonq/rules/coding_conventions`
- Từng filename bên trong conventions cũ.
- Amazon Q rule path hoặc context path cũ.

Kiểm tra trong:

- `.kiro/agents/`
- `.kiro/prompts/`
- `.kiro/skills/`
- `.kiro/steering/`
- `.amazonq/cli-agents/`
- `.amazonq/prompts/`
- Các script review liên quan.
- Tài liệu hướng dẫn sử dụng AI của project.

Không scan production source nếu không cần thiết.

## Phân loại dependency

| File | Reference hiện tại | Mục đích | Cần migrate | Target đề xuất |
| ---- | ------------------ | -------- | ----------: | -------------- |

Phân loại:

- `ACTIVE`: Workflow hiện tại vẫn sử dụng.
- `LEGACY`: Chỉ còn phục vụ Amazon Q.
- `DOCUMENTATION`: Chỉ xuất hiện trong tài liệu.
- `UNKNOWN`: Chưa xác định được vai trò.

## Kế hoạch archive

Đề xuất cách bảo toàn `.amazonq/rules/coding_conventions/` nhưng loại nó khỏi runtime context.

Yêu cầu:

1. Không xóa ngay.
2. Không để bản archive trong đường dẫn mà Kiro hoặc agent có thể tự động nạp.
3. Giữ được lịch sử và khả năng đối chiếu.
4. Không duplicate knowledge base trong runtime.
5. Cung cấp đường dẫn archive đề xuất.
6. Kiểm tra các glob resource để chắc chắn archive không bị match.

## Kết quả

Cung cấp:

1. Dependency matrix.
2. Danh sách reference cần migrate.
3. Kế hoạch archive.
4. Danh sách file sẽ thay đổi.
5. Các rủi ro.
6. Checklist xác nhận migration hoàn tất.
7. Chờ tôi phê duyệt; chưa thực hiện thay đổi.
