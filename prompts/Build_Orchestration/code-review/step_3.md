Hãy đọc báo cáo audit và remediation plan vừa tạo.

Bây giờ hỗ trợ tôi xử lý các mục `HUMAN_DECISION`, ưu tiên theo thứ tự:

1. `P0`
2. `P1`
3. `P2`
4. `P3`

Chưa được chỉnh sửa steering hoặc source code trong bước này.

## Cách thực hiện

Chỉ trình bày một decision tại một thời điểm.

Với mỗi decision, cung cấp ngắn gọn:

1. Finding ID.
2. Chủ đề.
3. Convention cũ đang quy định gì.
4. Steering hiện tại đang quy định gì.
5. Điểm khác biệt hoặc xung đột.
6. Ảnh hưởng nếu giữ convention cũ.
7. Ảnh hưởng nếu giữ steering hiện tại.
8. Các phương án lựa chọn:
   - Option A.
   - Option B.
   - Option C nếu thực sự cần.

9. Phương án được đề xuất.
10. Lý do đề xuất.
11. Câu hỏi cụ thể để tôi lựa chọn.

## Nguyên tắc đề xuất

Áp dụng thứ tự ưu tiên:

1. Phù hợp với kiến trúc và workflow thực tế của NBE.
2. Tương thích Angular 16.
3. Bảo toàn project-specific convention.
4. Không tự động áp dụng Angular official best practice nếu làm thay đổi convention hiện tại.
5. Ưu tiên giải pháp rõ ràng, có thể dùng cho cả implement và code review.
6. Không dùng code legacy làm căn cứ quyết định.

## Quy trình hội thoại

1. Bắt đầu với finding `P0` quan trọng nhất.
2. Chờ tôi trả lời trước khi chuyển sang finding tiếp theo.
3. Sau mỗi câu trả lời, nhắc lại quyết định đã được ghi nhận trong một dòng.
4. Không hỏi lại decision đã được xác nhận.
5. Không tự sửa file trong quá trình hỏi.
6. Nếu câu trả lời của tôi chưa đủ rõ, hỏi lại đúng một câu ngắn.

## Sau khi hoàn tất P0 và P1

Tạo decision register dạng bảng:

| Finding ID | Priority | Decision | Final rule | Reason | Affected steering files |
| ---------- | -------- | -------- | ---------- | ------ | ----------------------- |

Sau đó:

1. Liệt kê những finding đã được quyết định.
2. Liệt kê những finding còn chờ quyết định.
3. Đề xuất phạm vi chính xác của Batch 1.
4. Chờ tôi xác nhận trước khi chỉnh sửa steering.
