Hãy đọc:

1. Báo cáo audit ban đầu.
2. Remediation plan.
3. Decision register.
4. Kết quả và diff đã được user xác nhận của Batch 1, 2 và 3.
5. Các targeted re-audit tương ứng.

Bây giờ thực hiện `Batch 4 — Consolidation and token optimization`.

## Điều kiện bắt đầu

Chỉ thực hiện khi:

- Batch 1, 2 và 3 đã được user xác nhận.
- Không còn P0/P1 unresolved.
- Activation của steering đã ổn định.
- Working tree không có thay đổi ngoài phạm vi đã biết.

Nếu chưa đáp ứng, dừng và báo lại.

## Mục tiêu

1. Loại bỏ rule trùng lặp.
2. Xác định một nơi sở hữu duy nhất cho mỗi convention.
3. Chuẩn hóa terminology và mức độ bắt buộc.
4. Rút gọn nội dung không cần thiết.
5. Giảm context/token khi steering được kích hoạt.
6. Không làm mất hoặc thay đổi ý nghĩa bất kỳ convention nào đã được phê duyệt.

## Phạm vi

Chỉ xử lý:

- `DUPLICATE`.
- Nội dung chồng chéo.
- Câu chữ dài dòng.
- Ví dụ lặp lại.
- Heading hoặc cấu trúc khó tra cứu.
- Terminology không nhất quán.
- Nội dung giải thích không cần thiết cho việc implement hoặc review.

Không thay đổi:

- Quyết định trong decision register.
- Ý nghĩa rule.
- Mức độ `MUST`, `MUST NOT`, `SHOULD`, `MAY`.
- Phạm vi áp dụng.
- Angular 16 constraint.
- Project-specific convention.
- `inclusion` và `fileMatchPattern` đã duyệt ở Batch 3, trừ khi user quyết định riêng.
- Source code, agent, skill và prompt review.

## Trước khi chỉnh sửa

### 1. Đo baseline

Cung cấp cho từng steering file:

| File | Số dòng | Số từ hoặc ký tự | Số rule | Số duplicate |
| ---- | ------: | ---------------: | ------: | -----------: |

### 2. Tạo ownership matrix

Với mỗi duplicate:

| Rule ID | Các file đang chứa | Owner file đề xuất | Nội dung giữ lại | Nội dung loại bỏ |
| ------- | ------------------ | ------------------ | ---------------- | ---------------- |

Nguyên tắc chọn owner:

1. File có scope phù hợp nhất.
2. File có activation pattern đúng nhất.
3. Không chuyển rule sang file khiến nó được kích hoạt sai phạm vi.
4. Một rule chỉ có một owner chính.
5. Không dùng reference mơ hồ như “follow the rules above” hoặc “same as other file”.

### 3. Tạo semantic preservation matrix

| Rule ID | Nội dung trước | Nội dung đề xuất | MUST level | Scope | Có mất thông tin? |
| ------- | -------------- | ---------------- | ---------- | ----- | ----------------- |

Nếu không chứng minh được semantic equivalence, không được rút gọn rule đó.

## Quy tắc tối ưu

1. Dùng câu mệnh lệnh ngắn và trực tiếp.
2. Giữ rõ các từ `MUST`, `MUST NOT`, `SHOULD`, `MAY`.
3. Không thay rule cụ thể bằng câu chung chung.
4. Không bỏ:
   - Điều kiện áp dụng.
   - Ngoại lệ.
   - Project-specific utility hoặc abstraction bắt buộc.
   - Constraint về Angular version.

5. Chỉ giữ ví dụ khi:
   - Rule khó hiểu nếu không có ví dụ.
   - Có nhiều cách viết nhưng project chỉ chấp nhận một cách.
   - Ví dụ thể hiện pattern đặc thù của NBE.

6. Không giữ nhiều ví dụ minh họa cùng một rule.
7. Không đưa kiến thức Angular phổ thông vào steering nếu model đã biết và project không có customization.
8. Không thêm phần giải thích lịch sử hoặc lý do dài nếu không ảnh hưởng quyết định implement.
9. Không duplicate rule giữa phần overview và phần chi tiết.
10. Không tạo reference mới nếu nội dung đủ ngắn để giữ trực tiếp.
11. Không gộp các steering có activation scope khác nhau chỉ để giảm số file.

## Thực hiện

1. Chỉ sửa các steering đã được liệt kê trước.
2. Loại bỏ duplicate sau khi xác định owner.
3. Chuẩn hóa terminology.
4. Rút gọn wording nhưng bảo toàn semantic.
5. Giữ nguyên front matter đã duyệt ở Batch 3.
6. Không đổi tên hoặc di chuyển file nếu chưa có quyết định rõ ràng.
7. Không tự động xóa file steering.
8. Nếu một file trở nên không còn nội dung hữu ích, báo lại thay vì tự xóa.

## Validation

Sau khi cập nhật:

1. Kiểm tra diff chỉ gồm file Batch 4.
2. Map mọi thay đổi với Rule ID hoặc duplicate finding.
3. Thực hiện full semantic comparison trước và sau.
4. Kiểm tra:
   - Không mất convention.
   - Không thay đổi mức độ bắt buộc.
   - Không thay đổi scope.
   - Không tạo activation gap.
   - Không tạo conflict mới.
   - Mỗi rule có owner rõ ràng.
   - Không còn duplicate đã xử lý.

5. Đo lại số dòng, số từ hoặc ký tự.
6. Không chạy unit test hoặc Angular build.

## Báo cáo token optimization

Cung cấp:

| File | Trước | Sau | Giảm | Tỷ lệ giảm |
| ---- | ----: | --: | ---: | ---------: |

Không coi tỷ lệ giảm cao là thành công nếu semantic coverage bị giảm.

## Kết quả đầu ra

Cung cấp:

1. Danh sách duplicate đã xử lý.
2. Ownership matrix cuối cùng.
3. Danh sách file đã thay đổi.
4. Diff để user review.
5. Semantic preservation matrix.
6. Thống kê trước và sau.
7. Rule không thể rút gọn an toàn.
8. File có thể cân nhắc xóa hoặc gộp, nhưng chưa được tự xử lý.
9. Xác nhận front matter không bị thay đổi.
10. Chờ user xác nhận; không tự chuyển sang Batch 5.
