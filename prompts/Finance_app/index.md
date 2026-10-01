Trong thread này, chúng ta chỉ nói về ý tưởng cho project chứ chưa thực hiện tác động gì đến code nhé.
Ý tưởng là xây dựng một project quản lý tài chính đơn giản dùng cho chính tôi. Hãy cùng tôi phân tích bài toán này nhé:

Hiện trạng:
Tôi đang nắm giữ tiền gửi của khá nhiều người cho việc đầu tư chứng khoán.

- Nguồn tiền có thể rút ra rút vào tuỳ lúc dành cho từng người. (Bài toán chính)
  Ví dụ:
- Ngày 1 tháng 8:

* Tôi có nhận của A 100 triệu
* Tôi có nhận của B 200 triệu
* Tôi có 50 triệu

=> Tổng có 350 triệu

Sau đó tôi mang đi đầu tư (Có thể lãi hoặc lỗ)

- Ngày 15 tháng 10: Hiện tại đang lãi 30 triệu
  A có nhu cầu rút 20 triệu => Còn 80 triệu
  B có nhu cầu rút 10 triệu => Còn 190 triệu

Vì tiền đang mua cổ phiếu hết rồi nên tôi phải bán bớt 1 phần cổ phiếu để cho A và B rút số tiền trên. còn số còn lại vẫn giữ nguyên trong cổ phiếu để đầu tư tiếp.

=> Chúng ta cần phải tính lại tài sản cho mỗi người.

Tương tự nếu với bài toán bị lỗ cũng vậy.

Mong muốn:

- Thống kê tài sản, nguồn tiền rõ ràng
- Thống kê tổng tài sản hiện tại và tài sản cho từng người
- Quản lý các khoản nợ của tôi có thể đi vay (Kêu gọi vốn từ nguồn khác)

bây giờ, thời gian build của hệ thống đang rất lâu - rơi vào khoảng 30 phút và bundle size thì đang rất lớn dẫn đến một số máy chạy đang bị heap of memory. chúng ta cần review lại toàn bộ app về perfomance, app bundle, ... và lên plan để update hệ thống. Hãy tạo cho tôi prompt với kiro nhé
