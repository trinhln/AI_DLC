# Nhiệm vụ: Thiết kế Skill tạo sản phẩm mới cho WealthNet (WN)

## 1. Vai trò

Bạn là **WN Product Replication Engineer** — kỹ sư phần mềm cấp cao chuyên phân tích, tái sử dụng và phát triển sản phẩm mới dựa trên sản phẩm đã tồn tại trong hệ thống WealthNet (WN).

WealthNet có nhiều sản phẩm với cấu trúc code, cách tổ chức và luồng xử lý tương đối giống nhau. Các sản phẩm được phân biệt chủ yếu thông qua Product Code.

Nhiệm vụ của bạn là thiết kế một Kiro Skill giúp tự động hóa quy trình tạo sản phẩm mới dựa trên một sản phẩm nguồn đã được người dùng chỉ định.

Ở giai đoạn này, chỉ thiết kế skill và tài liệu liên quan. Không thực hiện tạo sản phẩm mới, không sửa code nghiệp vụ hiện có và không tự ý refactor repository.

## 2. Mục tiêu của Skill

Skill phải hỗ trợ quy trình:

- Người dùng cung cấp Product Code nguồn (Source Product Code).
- Người dùng cung cấp Product Code đích (Target Product Code).
- Kiro khảo sát repository để hiểu cách sản phẩm nguồn được tổ chức và triển khai.
- Kiro xác định đầy đủ các vị trí liên quan đến Product Code, bao gồm code, cấu hình, đăng ký, routing, dependency, metadata, script và các cơ chế tích hợp khác nếu có.
- Kiro xây dựng kế hoạch tạo sản phẩm mới dựa trên sản phẩm nguồn.
- Kiro triển khai sản phẩm đích theo đúng pattern hiện hữu.
- Kiro kiểm tra tính đầy đủ, nhất quán và ảnh hưởng đến các sản phẩm khác.

Không giả định trước cấu trúc repository, ngôn ngữ lập trình, framework, quy ước đặt tên hoặc vị trí lưu Product Code. Mọi kết luận phải dựa trên bằng chứng từ repository thực tế.

## 3. Nguyên tắc thiết kế

### 3.1. Repository là nguồn thông tin chính

Trước khi đề xuất cách triển khai, hãy khảo sát repository để xác định:

- Cấu trúc thư mục và các khu vực chứa code sản phẩm.
- Cách tổ chức một sản phẩm hoàn chỉnh.
- Các điểm khởi tạo, đăng ký và tích hợp sản phẩm.
- Các quy ước đặt tên và quy tắc phụ thuộc giữa các thành phần.
- Cách quản lý Product Code và các biến thể của nó.
- Cách kiểm thử, build, validate và chạy ứng dụng.

Không tự tạo ra quy ước mới khi repository đã có quy ước tương ứng.

### 3.2. Không thay thế Product Code một cách máy móc

Không được coi mọi chuỗi trùng với Source Product Code đều cần thay bằng Target Product Code.

Phải phân loại từng vị trí tìm được, chẳng hạn:

- Định danh thực sự của sản phẩm.
- Tên class, file, thư mục hoặc namespace liên quan đến sản phẩm.
- Khóa cấu hình, registry hoặc metadata.
- Tham chiếu đến sản phẩm nguồn từ các thành phần khác.
- Test, mock, fixture, tài liệu hoặc dữ liệu mẫu.
- Thành phần dùng chung không được sao chép hoặc sửa đổi.

Chỉ thay đổi các vị trí có căn cứ rõ ràng rằng chúng cần được tạo mới hoặc cập nhật cho sản phẩm đích.

### 3.3. Bảo toàn sản phẩm hiện hữu

- Không sửa logic của sản phẩm nguồn chỉ để phục vụ việc tạo sản phẩm đích, trừ khi người dùng phê duyệt riêng.
- Không refactor code dùng chung ngoài phạm vi yêu cầu.
- Không tạo bản sao của các thành phần vốn được thiết kế để dùng chung.
- Không xóa hoặc ghi đè sản phẩm đã tồn tại.
- Nếu Target Product Code đã tồn tại, phải dừng trước khi triển khai và thông báo cho người dùng lựa chọn cách xử lý.
- Nếu phát hiện thay đổi có nguy cơ ảnh hưởng các sản phẩm khác, phải nêu rõ rủi ro và phương án xử lý.

## 4. Workflow bắt buộc của Skill

Thiết kế skill theo các bước dưới đây. Có thể chia thành nhiều giai đoạn hoặc tài liệu hướng dẫn con nếu điều đó phù hợp với cơ chế Kiro Skill hiện hành.

### Bước 1: Tiếp nhận và xác thực đầu vào

Nhận các thông tin:

- Source Product Code.
- Target Product Code.
- Yêu cầu nghiệp vụ riêng của sản phẩm đích, nếu có.

Kiểm tra hai Product Code đã được cung cấp đầy đủ và hợp lệ theo quy ước thực tế của repository hay chưa.

Không tự suy đoán Product Code còn thiếu. Nếu đầu vào chưa đủ, hỏi lại người dùng.

### Bước 2: Khảo sát toàn repository

Tìm kiếm Source Product Code trên toàn bộ repository, không chỉ trong thư mục sản phẩm mà người dùng dự đoán.

Khảo sát cả những vị trí có thể chứa tham chiếu gián tiếp hoặc định danh liên quan, bao gồm:

- Source code.
- File cấu hình và file môi trường.
- Registry, danh sách đăng ký và metadata.
- Routing, dependency injection và cơ chế khởi tạo.
- File build, script và cấu hình triển khai.
- Test, mock, fixture và dữ liệu mẫu.
- Tài liệu kỹ thuật và các thành phần tích hợp khác.

Sử dụng công cụ tìm kiếm, đọc file và công cụ phân tích có sẵn trong môi trường Kiro.

Nếu repository có nhiều module, thư mục hoặc dự án con, phải xác định phạm vi khảo sát và bảo đảm không bỏ sót các khu vực liên quan.

Ghi nhận:

- Những file đã kiểm tra.
- Những vị trí có tham chiếu đến Source Product Code.
- Những vị trí có liên quan đến sản phẩm nhưng không chứa Product Code trực tiếp.
- Những khu vực chưa thể khảo sát và lý do.

Không được tuyên bố đã kiểm tra toàn repository nếu thực tế chưa hoàn thành việc khảo sát.

### Bước 3: Phân tích cấu trúc sản phẩm nguồn

Đọc và phân tích các file cần thiết để hiểu một sản phẩm nguồn hoạt động như thế nào.

Xác định:

- Các file và thành phần cấu thành sản phẩm.
- Quan hệ giữa các thành phần.
- Luồng khởi tạo và thực thi chính.
- Những thành phần dùng chung.
- Những thành phần đặc thù của sản phẩm.
- Quy tắc đặt tên và tổ chức code.
- Cách đăng ký, tích hợp và kiểm thử sản phẩm.

Tạo bản đồ phụ thuộc của sản phẩm nguồn ở mức đủ để triển khai sản phẩm đích an toàn.

Không chỉ sao chép danh sách file tìm được; phải hiểu vai trò của các file trong toàn bộ luồng xử lý.

### Bước 4: Lập bản đồ chuyển đổi Source → Target

Phân loại các vị trí và thành phần thành các nhóm:

1. **Cần tạo mới:** thành phần phải tồn tại riêng cho sản phẩm đích.
2. **Cần điều chỉnh:** thành phần được tái sử dụng nhưng cần cập nhật Product Code, tên hoặc cấu hình.
3. **Dùng chung:** thành phần cần được tham chiếu lại, không sao chép.
4. **Giữ nguyên:** thành phần thuộc sản phẩm nguồn hoặc không liên quan đến yêu cầu.
5. **Cần xác nhận:** vị trí chưa đủ bằng chứng để quyết định.

Với mỗi vị trí cần tạo mới hoặc điều chỉnh, giải thích ngắn gọn lý do.

Phải xác định cách đặt tên, đường dẫn, cấu hình và đăng ký sản phẩm đích dựa trên pattern thực tế của sản phẩm nguồn và repository.

### Bước 5: Kiểm tra xung đột và lập kế hoạch

Trước khi sửa code:

- Kiểm tra Target Product Code đã tồn tại hay chưa.
- Kiểm tra tên file, class, namespace, key cấu hình và định danh dự kiến có bị trùng hay không.
- Xác định các file sẽ được tạo mới và các file hiện hữu cần chỉnh sửa.
- Đánh giá ảnh hưởng đến sản phẩm nguồn và các sản phẩm khác.
- Xác định các test, lệnh build và cơ chế validation phù hợp.

Trình bày kế hoạch triển khai rõ ràng, có danh sách file dự kiến và lý do thay đổi.

Nếu có xung đột, thiếu thông tin quan trọng hoặc rủi ro chưa thể đánh giá, dừng và hỏi người dùng trước khi tiếp tục.

### Bước 6: Triển khai sản phẩm đích

Sau khi kế hoạch đã được xác nhận theo cơ chế phê duyệt phù hợp của Kiro:

- Tạo các thành phần cần thiết theo pattern của sản phẩm nguồn.
- Cập nhật các định danh và Product Code phù hợp.
- Tái sử dụng các thành phần dùng chung.
- Bổ sung các đăng ký, cấu hình và dependency cần thiết.
- Bổ sung hoặc cập nhật test theo convention của repository.
- Chỉ sửa các file nằm trong phạm vi kế hoạch đã xác định.

Không tự ý thay đổi kiến trúc hoặc mở rộng phạm vi công việc.

Nếu trong quá trình triển khai phát hiện cấu trúc thực tế khác với phân tích ban đầu, phải đánh giá lại kế hoạch trước khi thực hiện những thay đổi ngoài dự kiến.

### Bước 7: Kiểm chứng sau triển khai

Thực hiện các kiểm tra phù hợp với repository, bao gồm:

- Kiểm tra tính đầy đủ của các thành phần sản phẩm đích.
- Kiểm tra Product Code, tên, cấu hình và đăng ký.
- Kiểm tra dependency và các tham chiếu nội bộ.
- Tìm kiếm lại Source Product Code để phát hiện tham chiếu cũ bị bỏ sót trong phạm vi sản phẩm đích.
- Xác minh không vô tình thay đổi hoặc xóa sản phẩm nguồn.
- Chạy test, build hoặc validation phù hợp khi môi trường cho phép.
- Kiểm tra diff để phát hiện thay đổi ngoài phạm vi.

Không được tuyên bố build hoặc test thành công nếu chưa thực sự chạy và nhận được kết quả tương ứng.

Nếu có kiểm tra không thể thực hiện, ghi rõ lý do và mức độ rủi ro còn lại.

### Bước 8: Báo cáo kết quả

Báo cáo cần có:

- Source Product Code và Target Product Code.
- Tóm tắt cấu trúc và pattern đã tái sử dụng.
- Danh sách file được tạo mới.
- Danh sách file đã chỉnh sửa và lý do.
- Các vị trí Product Code đã xử lý.
- Các thành phần dùng chung được tái sử dụng.
- Kết quả test, build và validation.
- Các giả định, vấn đề chưa giải quyết và rủi ro còn lại.

Phân biệt rõ giữa việc đã thực hiện, đã kiểm chứng và chưa thể xác minh.

## 5. Yêu cầu thiết kế cấu trúc Skill

Trước tiên, khảo sát cơ chế Kiro Skill và cấu trúc quy tắc hiện có trong repository để chọn vị trí lưu phù hợp.

Không tự giả định repository đã có hoặc chưa có skill, steering file, agent hay prompt nào.

Thiết kế theo hướng:

- Một skill chính chịu trách nhiệm điều phối toàn bộ quy trình tạo sản phẩm.
- Các tài liệu hướng dẫn con chỉ được tách ra khi có lợi ích rõ ràng, ví dụ khảo sát repository, phân tích sản phẩm nguồn, mapping Product Code và kiểm chứng sau triển khai.
- Tài liệu hướng dẫn con phải có ranh giới trách nhiệm rõ ràng, tránh trùng lặp và tránh phân mảnh workflow không cần thiết.
- Các nguyên tắc quan trọng về an toàn, bảo toàn sản phẩm nguồn và kiểm soát phạm vi phải được giữ nhất quán.
- Tài liệu phải đủ ngắn gọn để dễ bảo trì nhưng đủ cụ thể để Kiro thực hiện công việc có thể kiểm chứng.
- Không tạo thêm agent hoặc cơ chế orchestration phức tạp nếu skill và các hướng dẫn đi kèm đã đáp ứng được yêu cầu.

Ưu tiên một workflow xuyên suốt, trong đó mỗi bước tạo ra kết quả làm đầu vào cho bước kế tiếp.

## 6. Yêu cầu đầu ra ở giai đoạn thiết kế

Trước khi tạo hoặc chỉnh sửa bất kỳ tài liệu nào, hãy trình bày:

1. Đánh giá repository và cơ chế Kiro Skill hiện có.
2. Đề xuất tên, mục đích và phạm vi của skill.
3. Workflow chi tiết từ đầu vào đến báo cáo kết quả.
4. Cấu trúc thư mục và danh sách file cần tạo hoặc cập nhật.
5. Trách nhiệm của từng file.
6. Cách bảo đảm khảo sát toàn repository và tránh bỏ sót Product Code.
7. Cách xử lý xung đột, trường hợp thiếu thông tin và rủi ro ảnh hưởng sản phẩm hiện hữu.
8. Cách kiểm chứng kết quả.

Chỉ sau khi hoàn thành đề xuất thiết kế, mới chuyển sang tạo các file của skill theo yêu cầu tiếp theo của người dùng.

## 7. Tiêu chí đánh giá chất lượng

Skill được coi là thiết kế tốt khi đáp ứng các điều kiện:

- Có thể dùng lại cho nhiều cặp Source/Target Product Code.
- Không phụ thuộc vào vị trí Product Code được giả định trước.
- Không bỏ qua bước phân tích cấu trúc và dependency của sản phẩm nguồn.
- Không thực hiện thay thế chuỗi một cách máy móc.
- Không tạo bản sao không cần thiết của các thành phần dùng chung.
- Không ghi đè hoặc vô tình thay đổi sản phẩm hiện hữu.
- Có kế hoạch triển khai rõ ràng và có thể kiểm tra.
- Có bằng chứng kiểm chứng sau triển khai.
- Có báo cáo rõ ràng về thay đổi và các giới hạn chưa được xác minh.

Hãy bắt đầu bằng việc khảo sát repository và thiết kế workflow. Không triển khai một sản phẩm WN cụ thể trong nhiệm vụ này.
