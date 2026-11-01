# NHIỆM VỤ: PHÂN TÍCH AI CODEGUARD VÀ XÂY DỰNG TÀI LIỆU MÔ TẢ SÁNG KIẾN

## 1. Mục tiêu

Phân tích repository hiện tại để hiểu đầy đủ ý tưởng, cách thức hoạt động và giá trị của giải pháp AI CodeGuard.

Từ kết quả phân tích, xây dựng một tài liệu mô tả sáng kiến phục vụ đăng ký tham gia cuộc thi AI Spot.

Mục tiêu không phải là xây dựng tài liệu đặc tả kỹ thuật chi tiết, mà là giúp người đọc, đặc biệt là ban giám khảo, hiểu được:

- AI CodeGuard giải quyết vấn đề gì?
- Ý tưởng và cách tiếp cận của giải pháp là gì?
- Giải pháp hoạt động như thế nào?
- AI được ứng dụng ở những công đoạn nào?
- Giá trị và lợi ích mà giải pháp mang lại?
- Khả năng áp dụng và mở rộng trong tổ chức?

## 2. Khám phá và phân tích Repository

Thực hiện tìm kiếm trên toàn bộ repository để xác định các thành phần liên quan đến AI CodeGuard.

Ưu tiên phân tích:

- Các Prompt phục vụ Code Review.
- Kiro Steering và Coding Convention.
- Các Kiro Skills liên quan.
- Các file cấu hình và quy tắc đánh giá.
- Quy trình thực thi Code Review.
- Cơ chế phân tích, đánh giá và tính điểm chất lượng code.
- Cấu trúc báo cáo kết quả review.

Không chỉ đọc tên file. Cần đọc nội dung thực tế và truy vết mối quan hệ giữa các thành phần.

### Những nội dung cần làm rõ

1. Ý tưởng cốt lõi và nguyên lý hoạt động của AI CodeGuard.
2. Quy trình Code Review từ đầu vào đến đầu ra.
3. Vai trò của Prompt, Steering và Skills trong giải pháp.
4. Cách AI tiếp nhận và áp dụng Coding Convention của dự án.
5. Các tiêu chí AI sử dụng để phân tích và đánh giá code.
6. Cách thức tạo kết quả review và đánh giá chất lượng.
7. Những điểm khác biệt so với Code Review thủ công truyền thống.

Tập trung vào việc hiểu giải pháp ở góc độ tổng thể, không cần phân tích chi tiết từng dòng code hoặc lập inventory toàn bộ repository.

## 3. Xây dựng tài liệu mô tả sáng kiến

Sau khi hoàn thành phân tích, tạo tài liệu:

`docs/ai-spot/ai-codeguard-initiative.md`

Tài liệu cần được viết bằng tiếng Việt, sử dụng ngôn ngữ chuyên nghiệp, dễ hiểu và hướng đến giá trị kinh doanh.

Cấu trúc đề xuất:

### 3.1. Tổng quan sáng kiến

- Initiative Name: AI CodeGuard.
- Brief Description.
- Mục tiêu của sáng kiến.

### 3.2. Bài toán cần giải quyết

- Những hạn chế của Code Review truyền thống.
- Các khó khăn về thời gian, chất lượng và tính nhất quán.
- Lý do cần ứng dụng AI.

### 3.3. Ý tưởng và giải pháp

- Ý tưởng cốt lõi của AI CodeGuard.
- Cách AI tham gia vào quy trình Code Review.
- Cách giải pháp sử dụng các quy chuẩn riêng của dự án.
- Điểm khác biệt so với cách review thông thường.

### 3.4. Cách thức hoạt động

Mô tả quy trình tổng thể từ khi bắt đầu review đến khi nhận được kết quả.

Có thể sử dụng sơ đồ Mermaid nếu giúp người đọc dễ hình dung.

Không sa đà vào chi tiết kỹ thuật không cần thiết.

### 3.5. Giá trị và lợi ích

- Tiết kiệm thời gian review.
- Nâng cao chất lượng mã nguồn.
- Chuẩn hóa Coding Convention.
- Giảm tải công việc cho Developer và Reviewer.
- Tăng năng suất đội ngũ phát triển.

### 3.6. Đối tượng hưởng lợi (Beneficiaries)

- Developers.
- Technical Leads / Code Reviewers.
- Development Teams.
- Project / Organization.

### 3.7. Tác động gia tăng (Incremental Impact)

Giải thích giá trị mới mà AI mang lại so với quy trình hiện tại, không chỉ dừng ở việc tự động hóa thao tác.

### 3.8. Kết quả và hiệu quả có thể đo lường

Đề xuất các KPI phù hợp, ví dụ:

- Thời gian Code Review.
- Thời gian review thủ công.
- Thời gian phản hồi.
- Năng suất Developer.

Mục tiêu kỳ vọng giảm 30–50% thời gian Code Review.

Lưu ý: Đây là mục tiêu kỳ vọng, không được trình bày như kết quả đã đo lường thực tế.

### 3.9. Khả năng mở rộng

Phân tích khả năng áp dụng cho nhiều dự án, đội ngũ và Tech Stack khác nhau thông qua việc tái sử dụng và tùy chỉnh Prompt, Steering, Skills và Coding Convention.

### 3.10. Định hướng phát triển

Nêu một số hướng mở rộng có tiềm năng trong tương lai, phân biệt rõ với chức năng hiện tại.

## 4. Nguyên tắc quan trọng

- Tài liệu phải phản ánh đúng ý tưởng và implementation hiện có.
- Không tự suy diễn những tính năng chưa được triển khai.
- Không phóng đại hiệu quả hoặc tự tạo số liệu thực tế.
- Phân biệt rõ khả năng hiện tại, mục tiêu kỳ vọng và định hướng tương lai.
- Không đưa các vấn đề kỹ thuật nội bộ không liên quan đến giá trị sáng kiến vào tài liệu chính.
- Ưu tiên cách diễn đạt dễ hiểu với người không có chuyên môn sâu về AI hoặc lập trình.
- Nội dung phải nhất quán với những gì đang có trong repository.

## 5. Phạm vi chỉnh sửa

- Chỉ phân tích repository.
- Không thay đổi Prompt, Steering, Skills, source code hoặc cấu hình hiện có.
- Chỉ tạo tài liệu `docs/ai-spot/ai-codeguard-initiative.md`.
- Nếu thiếu thông tin để mô tả chính xác ý tưởng, hãy ghi nhận thành câu hỏi cần xác nhận thay vì tự đưa ra giả định.

## 6. Kết quả mong đợi

Sau khi hoàn thành, cung cấp:

1. Tóm tắt cách AI CodeGuard hoạt động theo phân tích repository.
2. Những điểm quan trọng đã sử dụng để xây dựng tài liệu.
3. Đường dẫn tài liệu đã tạo.
4. Những thông tin còn thiếu cần con người xác nhận.

Bắt đầu bằng việc khám phá và phân tích repository, sau đó trực tiếp xây dựng tài liệu mô tả sáng kiến.
