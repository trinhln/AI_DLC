# Kiến trúc Phalcon kết hợp Angular

## 1. Phalcon là hệ thống chính

Phalcon tiếp tục đảm nhiệm:

- Backend và nghiệp vụ hệ thống.
- Kết nối database.
- Authentication và authorization.
- Render các trang client cần SEO.
- Cung cấp API dùng chung qua module `/api`.

Cấu trúc URL dự kiến:

- `/` — website client do Phalcon render.
- `/angular-admin/` — hệ thống quản trị Angular.
- `/api/` — API dùng chung.
- `/client-app/` — bundle Angular dành cho các trang client tương tác cao.

Tất cả có thể chạy trên cùng một domain.

## 2. Angular Admin

Sử dụng project `common-angular` làm Angular Admin:

- Phát triển độc lập với project Phalcon.
- Build với base path `/angular-admin/`.
- Đưa kết quả build vào `public/angular-admin` của project Phalcon.
- Gọi API qua các endpoint `/api/...`.
- Phalcon vẫn kiểm soát quyền truy cập và nghiệp vụ phía server.

Luồng xử lý:

Angular Admin → Phalcon API module → Services/Database

## 3. Angular Client

Tạo một project Angular client riêng nhưng không thay thế toàn bộ frontend Phalcon.

Angular Client chỉ phụ trách các trang cần tương tác cao, ví dụ:

- Tìm kiếm sản phẩm với nhiều bộ lọc.
- Giỏ hàng phức tạp.
- Checkout nhiều bước.
- Dashboard tài khoản.
- Cấu hình sản phẩm.
- Những màn hình cần quản lý nhiều trạng thái phía client.

Các trang quan trọng với SEO như trang chủ, category, chi tiết sản phẩm và landing page vẫn được Phalcon render HTML.

## 4. SEO và bộ lọc sản phẩm

Phalcon trả HTML có nội dung SEO trong lần truy cập đầu tiên.

Sau khi trang được tải:

- Angular khởi động trong vùng giao diện cần tương tác.
- Angular đọc path và query parameters.
- Angular gọi `/api/...` để lấy dữ liệu.
- Khi người dùng thay đổi bộ lọc, Angular cập nhật URL và kết quả mà không reload toàn bộ trang.

Ví dụ:

- `/products/laptop` — category chính, được index và đưa vào sitemap.
- `/products/laptop?brand=dell&ram=16` — trạng thái filter, thường không cần index.

Các URL filter có thể đặt canonical về URL category chính để tránh duplicate content.

## 5. Tổ chức nhiều trang Angular Client

Không cần tạo một folder build cho mỗi trang.

Chỉ cần một Angular Client application được build vào:

`public/client-app/`

Angular Router quản lý các feature:

- `/search`
- `/cart`
- `/checkout`
- `/account`
- `/product-configurator`

Phalcon ánh xạ những URL cần Angular về cùng Angular application. Angular Router quyết định component tương ứng.

## Kiến trúc tổng thể

Phalcon project
├── Frontend module — HTML, SEO và nội dung chính
├── API module — API dùng chung
├── public/angular-admin — Angular dành cho quản trị viên
└── public/client-app — Angular cho các trang client tương tác cao

Mục tiêu là giữ SEO và sự đơn giản của Phalcon, đồng thời sử dụng Angular tại những khu vực mà AJAX thuần sẽ khó phát triển và bảo trì.
