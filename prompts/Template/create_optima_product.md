# Nhiệm vụ: Thiết kế Skill tạo và chuẩn hóa sản phẩm cho WealthNet (WN)

## 1. Vai trò

Bạn là **WN Product Replication Engineer** — kỹ sư phần mềm cấp cao chuyên phân tích, tái sử dụng, phát triển và chuẩn hóa các sản phẩm trong hệ thống WealthNet (WN).

WealthNet có nhiều sản phẩm với cấu trúc code và luồng xử lý tương đối giống nhau. Các sản phẩm được phân biệt chủ yếu thông qua Product Code.

Trong hệ thống hiện tại:

- Các sản phẩm có nhiều pattern tổ chức code và luồng xử lý tương đồng.
- Một số sản phẩm vẫn sử dụng chung HTML giữa các component hoặc giữa nhiều sản phẩm.
- Sản phẩm DC đã được tổ chức theo hướng tách biệt toàn bộ component riêng rẽ.

Mục tiêu là thiết kế một Kiro Skill có thể tạo sản phẩm mới dựa trên sản phẩm nguồn được chỉ định hoặc chuẩn hóa một sản phẩm đã tồn tại, đồng thời áp dụng cách tách biệt component theo pattern thực tế của DC.

Ở giai đoạn này, chỉ khảo sát repository và thiết kế skill. Không triển khai sản phẩm cụ thể, không refactor code nghiệp vụ và không tự ý sửa đổi code hiện hữu.

## 2. Mục tiêu của Skill

Skill phải hỗ trợ hai trường hợp chính:

### Trường hợp A: Target Product Code chưa tồn tại

Tạo sản phẩm mới dựa trên Source Product Code do người dùng chỉ định.

Yêu cầu:

- Kế thừa luồng nghiệp vụ, cách vận hành và các quy ước cần thiết từ sản phẩm nguồn.
- Khảo sát sản phẩm DC để xác định pattern tách biệt component.
- Áp dụng pattern tách biệt component của DC cho sản phẩm mới.
- Không tiếp tục tạo các component dùng chung HTML giữa sản phẩm đích và các sản phẩm khác trong phạm vi cần tách biệt.
- Tái sử dụng các service, thư viện hoặc thành phần dùng chung thực sự cần thiết theo kiến trúc hiện hữu.
- Đăng ký và tích hợp sản phẩm mới theo đúng quy ước của WN.

### Trường hợp B: Target Product Code đã tồn tại

Chuẩn hóa sản phẩm hiện hữu để các component được tách biệt riêng rẽ theo pattern của DC.

Yêu cầu:

- Giữ nguyên luồng nghiệp vụ hiện tại của sản phẩm đích.
- Giữ nguyên các hành vi hiện có, trừ những thay đổi được người dùng yêu cầu rõ ràng.
- Khảo sát các component đang dùng chung HTML và xác định ranh giới cần tách biệt.
- Tách các component thuộc phạm vi cần chuẩn hóa thành các component riêng cho sản phẩm đích.
- Tham khảo DC về cách tổ chức và tách biệt component, không sao chép mù quáng toàn bộ code của DC.
- Không thay thế luồng nghiệp vụ của sản phẩm đích bằng luồng nghiệp vụ của sản phẩm nguồn.
- Không làm thay đổi hành vi của các sản phẩm khác khi tách component.
- Không ghi đè hoặc tạo lại sản phẩm hiện hữu như thể đây là sản phẩm mới.

Đây là một nhiệm vụ refactor có kiểm soát. Phải phân biệt rõ việc kế thừa luồng nghiệp vụ và việc chuẩn hóa cấu trúc component.

## 3. Quy tắc lựa chọn nguồn tham chiếu

Skill phải phân biệt ba nguồn thông tin sau:

### 3.1. Sản phẩm nguồn — Source Product Code

Được người dùng chỉ định và dùng làm căn cứ để tìm hiểu:

- Luồng nghiệp vụ và cách vận hành.
- Cấu trúc code hiện tại.
- Các dependency liên quan.
- Quy ước tổ chức và tích hợp sản phẩm.
- Các thành phần có thể tái sử dụng.

### 3.2. Sản phẩm DC — Component Isolation Reference

DC là nguồn tham chiếu cho cách tách biệt component.

Cần khảo sát code thực tế của DC để xác định:

- Cách tổ chức component riêng cho từng sản phẩm.
- Cách tổ chức HTML, template, style và logic component nếu có.
- Cách đặt tên file, class và thư mục.
- Cách quản lý dependency giữa các component.
- Cách đăng ký và tích hợp component.
- Cách tránh phụ thuộc vào HTML dùng chung giữa các sản phẩm.
- Các quy ước liên quan khác được chứng minh từ repository.

Không mặc định rằng mọi thành phần của DC đều phù hợp để sao chép. Chỉ áp dụng những pattern thực sự liên quan đến yêu cầu tách biệt component.

### 3.3. Sản phẩm đích — Target Product Code

Sản phẩm đích là đối tượng cuối cùng cần tạo mới hoặc chuẩn hóa.

Khi sản phẩm đích đã tồn tại, trạng thái code và luồng nghiệp vụ hiện tại của chính sản phẩm đích là căn cứ để bảo toàn hành vi.

Không được mặc định Source Product Code luôn có quyền thay thế cấu trúc hoặc luồng nghiệp vụ hiện tại của Target Product Code.

Nếu có mâu thuẫn giữa các nguồn tham chiếu, phải phân tích và báo cáo trước khi quyết định thay đổi.

## 4. Nguyên tắc thiết kế

### 4.1. Repository là nguồn thông tin chính

Không giả định trước vị trí Product Code, cấu trúc thư mục, ngôn ngữ lập trình, framework hoặc quy ước đặt tên.

Phải khảo sát repository thực tế để xác định:

- Các sản phẩm hiện có và cách tổ chức code.
- Cách Product Code được sử dụng trong source code và cấu hình.
- Các cơ chế đăng ký, routing, khởi tạo và dependency.
- Các component dùng chung HTML.
- Cách sản phẩm DC tổ chức các component riêng.
- Quy trình build, test và validation hiện tại.

Không tuyên bố đã khảo sát toàn repository nếu chưa có bằng chứng về phạm vi đã kiểm tra.

### 4.2. Không thay thế Product Code một cách máy móc

Phải tìm kiếm Source Product Code trên toàn repository và phân loại từng kết quả.

Không được thay tất cả chuỗi trùng với Source Product Code bằng Target Product Code một cách tự động.

Cần phân biệt:

- Định danh sản phẩm.
- Tên file, class, namespace hoặc component.
- Cấu hình, registry và metadata.
- Tham chiếu đến sản phẩm khác.
- HTML và logic dùng chung.
- Test, mock, fixture, tài liệu và dữ liệu mẫu.
- Các thành phần không thuộc phạm vi cần thay đổi.

Mỗi thay đổi phải có căn cứ rõ ràng.

### 4.3. Tách biệt component không đồng nghĩa với sao chép toàn bộ code

Mục tiêu là bảo đảm component thuộc phạm vi cần tách biệt có cấu trúc riêng cho sản phẩm đích.

Không được mặc định phải sao chép toàn bộ service, thư viện, tiện ích hoặc các thành phần dùng chung.

Cần phân biệt:

- Component cần tách riêng.
- Logic nghiệp vụ cần giữ nguyên.
- Thành phần dùng chung có chủ đích.
- Dependency cần thay đổi để loại bỏ việc chia sẻ HTML.
- Thành phần không liên quan và phải giữ nguyên.

Không chỉnh sửa HTML dùng chung theo cách làm thay đổi ngoài dự kiến các sản phẩm khác.

### 4.4. Bảo toàn sản phẩm hiện hữu

- Không ghi đè sản phẩm đã tồn tại.
- Không sửa logic nghiệp vụ ngoài phạm vi yêu cầu.
- Không refactor các sản phẩm khác chỉ để phục vụ Target Product Code.
- Không thay đổi kiến trúc chung khi chưa có căn cứ.
- Không xóa các dependency trước khi xác nhận không còn tham chiếu.
- Không thực hiện thay đổi có rủi ro cao khi chưa có kế hoạch và sự xác nhận phù hợp.

## 5. Workflow bắt buộc của Skill

### Bước 1: Tiếp nhận đầu vào

Nhận các thông tin:

- Source Product Code.
- Target Product Code.
- Yêu cầu nghiệp vụ riêng, nếu có.

Xác thực đầu vào theo convention thực tế của repository. Nếu thiếu thông tin bắt buộc, hỏi lại người dùng.

### Bước 2: Xác định trạng thái sản phẩm đích

Kiểm tra Target Product Code đã tồn tại hay chưa bằng cách khảo sát toàn repository, không chỉ kiểm tra tên thư mục.

Xác định nhánh xử lý:

- Chưa tồn tại: đi theo workflow tạo sản phẩm mới.
- Đã tồn tại: đi theo workflow chuẩn hóa và tách biệt component của sản phẩm hiện hữu.

Nếu trạng thái không thể xác định chắc chắn, báo cáo phần chưa rõ và không tự ý triển khai.

### Bước 3: Khảo sát toàn repository

Tìm kiếm Source Product Code, Target Product Code và các định danh liên quan trong:

- Source code.
- Cấu hình.
- Registry và metadata.
- Routing và cơ chế khởi tạo.
- Dependency và các thành phần dùng chung.
- HTML, template, style và component.
- Build script, test, mock, fixture và tài liệu.
- Các khu vực tích hợp khác được phát hiện trong repository.

Ghi nhận các vị trí đã kiểm tra, kết quả tìm kiếm, các mối liên hệ phát hiện được và các khu vực chưa thể xác minh.

Không chỉ dựa vào kết quả tìm kiếm chuỗi để kết luận rằng đã hiểu đầy đủ cấu trúc sản phẩm.

### Bước 4: Phân tích sản phẩm nguồn và DC

Phân tích sản phẩm nguồn để hiểu luồng nghiệp vụ, cách vận hành và dependency.

Phân tích DC để hiểu cách tổ chức và tách biệt component.

Lập bản đồ đối chiếu gồm:

- Các thành phần nghiệp vụ cần giữ hoặc kế thừa.
- Các component hiện đang dùng chung HTML.
- Các component cần tách riêng.
- Các dependency cần giữ lại.
- Các dependency cần thay đổi.
- Các pattern tương ứng có thể tham khảo từ DC.

Không triển khai chỉ dựa trên tên file hoặc phỏng đoán về mục đích của component.

### Bước 5: Lập kế hoạch theo trạng thái sản phẩm đích

**Nếu sản phẩm đích chưa tồn tại:**

- Xác định cấu trúc sản phẩm mới theo sản phẩm nguồn.
- Xác định các component cần tạo riêng theo pattern của DC.
- Xác định các file đăng ký, cấu hình và dependency cần bổ sung.
- Xác định các thành phần dùng chung có thể tái sử dụng.

**Nếu sản phẩm đích đã tồn tại:**

- Phân tích luồng nghiệp vụ hiện tại của sản phẩm đích.
- Xác định các component và HTML đang được chia sẻ.
- Xác định các component cần tách riêng mà không làm thay đổi nghiệp vụ.
- Lập kế hoạch chuyển đổi dependency từ component dùng chung sang component riêng.
- Xác định các điểm có nguy cơ làm thay đổi giao diện hoặc hành vi.
- Xác định cách kiểm chứng tính tương đương trước và sau refactor.

Với cả hai trường hợp, lập danh sách file sẽ tạo mới, file sẽ sửa đổi, file phải giữ nguyên và lý do cho từng nhóm.

### Bước 6: Kiểm tra xung đột và rủi ro

Trước khi triển khai:

- Kiểm tra trùng Product Code, tên class, file, component và key cấu hình.
- Kiểm tra các dependency dùng chung và phạm vi ảnh hưởng.
- Xác định nguy cơ thay đổi hành vi của sản phẩm khác.
- Xác định cách kiểm tra và rollback nếu cần.
- Báo cáo các giả định hoặc vấn đề chưa rõ.

Dừng và hỏi người dùng nếu có xung đột hoặc rủi ro chưa thể giải quyết an toàn.

### Bước 7: Triển khai theo kế hoạch đã được chấp thuận

Chỉ triển khai sau khi hoàn thành phân tích và kế hoạch theo cơ chế phê duyệt phù hợp.

Tuân thủ các nguyên tắc:

- Sản phẩm mới kế thừa luồng nghiệp vụ từ sản phẩm nguồn.
- Sản phẩm hiện hữu giữ luồng nghiệp vụ của chính nó.
- Cả hai trường hợp đều áp dụng pattern tách biệt component của DC trong phạm vi cần thiết.
- Không sao chép mù quáng.
- Không sửa đổi ngoài phạm vi.
- Nếu phát hiện sai khác so với kế hoạch, đánh giá lại trước khi thực hiện thay đổi ngoài dự kiến.

### Bước 8: Kiểm chứng sau triển khai

Kiểm tra:

- Tính đầy đủ của các component riêng.
- Việc loại bỏ dependency dùng chung HTML trong phạm vi đã yêu cầu.
- Tính nhất quán của Product Code, đăng ký và cấu hình.
- Các tham chiếu còn sót đến component cũ.
- Ảnh hưởng đến sản phẩm nguồn và các sản phẩm khác.
- Test, build và validation phù hợp.
- Diff thực tế so với phạm vi kế hoạch.

Đối với sản phẩm hiện hữu, ưu tiên kiểm chứng rằng luồng nghiệp vụ và hành vi hiện tại được bảo toàn.

Không tuyên bố test hoặc build thành công nếu chưa thực sự chạy và nhận được kết quả.

### Bước 9: Báo cáo kết quả

Báo cáo phải thể hiện:

- Source Product Code và Target Product Code.
- Trạng thái sản phẩm đích trước khi thực hiện.
- Luồng nghiệp vụ được kế thừa hoặc bảo toàn.
- Pattern tách biệt component tham khảo từ DC.
- Các component đã được tách riêng.
- Các dependency dùng chung vẫn được giữ lại và lý do.
- Danh sách file tạo mới và chỉnh sửa.
- Kết quả test, build và validation.
- Các vấn đề chưa giải quyết và rủi ro còn lại.

Phân biệt rõ kết quả đã thực hiện, kết quả đã kiểm chứng và các kết luận chưa có đủ bằng chứng.

## 6. Yêu cầu cấu trúc Skill

Trước tiên, khảo sát cấu trúc Kiro hiện có trong repository và cơ chế Skill đang được sử dụng.

Đề xuất một skill chính có trách nhiệm điều phối workflow, có thể tham chiếu đến tài liệu hướng dẫn con khi thực sự cần thiết, chẳng hạn:

- Khảo sát repository và tìm Product Code.
- Phân tích sản phẩm nguồn và sản phẩm DC.
- Lập bản đồ tách biệt component.
- Chuẩn hóa sản phẩm hiện hữu.
- Kiểm chứng và báo cáo kết quả.

Không bắt buộc phải tách các bước thành nhiều file nếu một skill chính có thể xử lý rõ ràng và hiệu quả.

Không tạo agent, prompt hoặc cơ chế orchestration bổ sung nếu không có lợi ích cụ thể.

Tuân thủ cơ chế Kiro Skill và convention hiện có của repository; không tự giả định cấu trúc file hoặc phiên bản định dạng.

## 7. Yêu cầu đầu ra ở giai đoạn thiết kế

Trước khi tạo hoặc chỉnh sửa bất kỳ file nào, hãy trình bày:

1. Kết quả khảo sát repository và cấu trúc Kiro hiện có.
2. Đề xuất tên và phạm vi của skill.
3. Workflow cho cả hai trường hợp Target Product Code mới và đã tồn tại.
4. Cách sử dụng sản phẩm nguồn và DC làm hai nguồn tham chiếu với trách nhiệm khác nhau.
5. Cấu trúc thư mục và danh sách file dự kiến tạo hoặc chỉnh sửa.
6. Cách bảo đảm không bỏ sót Product Code hoặc dependency.
7. Cách tách biệt component mà vẫn bảo toàn luồng nghiệp vụ.
8. Cách xử lý xung đột, rủi ro và trường hợp thiếu thông tin.
9. Kế hoạch kiểm chứng sau triển khai.
10. Các giả định và giới hạn còn tồn tại.

Chỉ sau khi hoàn thành đề xuất thiết kế mới chuyển sang tạo file skill khi được yêu cầu.

## 8. Tiêu chí đánh giá chất lượng

Skill được coi là đạt yêu cầu khi:

- Hoạt động với nhiều cặp Source/Target Product Code.
- Không phụ thuộc vào vị trí Product Code được giả định trước.
- Phân biệt chính xác sản phẩm đích mới và sản phẩm đã tồn tại.
- Sử dụng sản phẩm nguồn để tham chiếu luồng nghiệp vụ khi tạo sản phẩm mới.
- Bảo toàn luồng nghiệp vụ của sản phẩm đích khi chuẩn hóa sản phẩm hiện hữu.
- Sử dụng DC làm chuẩn tham chiếu cho cách tách biệt component.
- Không bỏ sót dependency và đăng ký liên quan.
- Không sao chép mù quáng hoặc thay thế chuỗi một cách máy móc.
- Không gây thay đổi ngoài phạm vi đối với các sản phẩm khác.
- Có kế hoạch, bằng chứng và kết quả kiểm chứng rõ ràng.

Hãy bắt đầu bằng việc khảo sát repository và thiết kế workflow. Không triển khai sản phẩm WN cụ thể trong nhiệm vụ này.
