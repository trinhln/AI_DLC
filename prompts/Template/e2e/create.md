# Nhiệm vụ: Phân tích solution E2E hiện tại và tạo bộ Kiro Skills để tự động sinh Playwright E2E Test

## 1. Vai trò

Bạn là Senior QA Automation Architect, có chuyên môn sâu về Angular, Playwright, UI Regression Testing và Kiro Agent Skills.

Nhiệm vụ của bạn là phân tích solution E2E hiện có trong repository, sau đó thiết kế và tạo một bộ Kiro Skills có thể tái sử dụng để tự động sinh Playwright E2E Test cho một màn hình Angular dựa trên đường dẫn source code của màn hình đó.

**File solution hiện tại là nguồn thông tin chuẩn** cho workflow E2E, chiến lược screenshot, cấu trúc đầu ra và các yêu cầu kiểm thử.

Không tự ý thay thế solution hiện tại bằng một kiến trúc khác. Nếu phát hiện hạn chế kỹ thuật thực sự, hãy phân tích bằng chứng, giải thích ảnh hưởng và đề xuất phương án trước khi thay đổi các nguyên tắc cốt lõi.

## 2. Phân tích tài liệu và repository trước khi thiết kế Skills

Đọc toàn bộ nội dung file solution tại đường dẫn:

`<SOLUTION_FILE_PATH>`

Không chỉ đọc phần tóm tắt hoặc các tiêu đề. Phải hiểu đầy đủ các yêu cầu, ví dụ, quy tắc chụp ảnh, cơ chế mock data, cấu trúc đầu ra, quy tắc validation và những ràng buộc đã được xác định.

Tiếp theo, kiểm tra repository hiện tại để hiểu:

- Phiên bản Angular và cấu trúc routing.
- Kiến trúc component, shared component, dialog, tab và các component được render động.
- Các Kiro Skills, steering files, agents và prompts hiện có.
- Cấu hình Playwright, fixtures, helpers, test conventions và cơ chế chụp screenshot.
- Cơ chế mock API, mock data, authentication và test utilities hiện có.
- Quy ước đặt tên file, tổ chức thư mục và các path alias của dự án.
- Các công cụ hoặc hạ tầng có thể tái sử dụng.

Ưu tiên tận dụng hạ tầng hiện có. Không tạo framework mới, helper trùng lặp hoặc nhiều cơ chế cạnh tranh nếu repository đã có giải pháp phù hợp.

Nếu solution và repository có điểm không tương thích, hãy chỉ ra cụ thể. Ưu tiên giữ nguyên ý đồ của solution, trừ khi có bằng chứng về một trở ngại kỹ thuật thực sự.

## 3. Mục tiêu chính

Tạo một bộ Skills để hỗ trợ workflow sau:

**Input:** Đường dẫn source code của một màn hình Angular.

**Workflow mong muốn:**

1. Phân tích màn hình mục tiêu và toàn bộ dependency liên quan.
2. Xác định route, entry component, child component, shared component, dialog, tab, UI có điều kiện và các component được khởi tạo động.
3. Phân tích template, TypeScript, styles, services, models, API và logic dữ liệu ảnh hưởng đến việc render UI.
4. Xác định các trạng thái UI cần kiểm tra và dữ liệu cần thiết để kích hoạt những trạng thái đó.
5. Tạo hoặc tái sử dụng mock data, mock API theo đúng contract và quy ước của repository.
6. Sinh Playwright E2E Test để truy cập màn hình và đi qua các trạng thái UI cần thiết.
7. Tương tác với các thành phần liên quan như dialog, tab, dropdown, menu và các control khác theo yêu cầu của solution.
8. Chụp screenshot cho màn hình và các trạng thái UI cần thiết theo đúng chiến lược được quy định trong file solution.
9. Chạy hoặc validation test khi môi trường cho phép.
10. Báo cáo phạm vi đã kiểm tra, kết quả, lỗi, dependency chưa giải quyết được và những trạng thái chưa thể bao phủ.

Mục tiêu không chỉ là xác nhận route truy cập thành công. Phải tạo được các trạng thái UI cần thiết để có thể kiểm tra giao diện một cách tự động và có thể tái lập.

## 4. Các năng lực bắt buộc của bộ Skills

Thiết kế số lượng Skills dựa trên solution và cấu trúc repository thực tế. Tách trách nhiệm khi cần thiết để tăng khả năng tái sử dụng, nhưng không chia nhỏ một cách máy móc.

### A. Phân tích màn hình và dependency

Khi nhận đường dẫn source code của màn hình, phải:

- Xác định component đầu vào và route tương ứng.
- Truy vết các import, template, stylesheet, child component và shared component liên quan.
- Phân tích quan hệ giữa các component và xác định những dependency thực sự ảnh hưởng đến UI.
- Tìm dialog component và cách chúng được mở.
- Xác định tab, dropdown, menu, conditional rendering và những trạng thái UI chỉ xuất hiện sau tương tác.
- Phân tích service, API, model, interface và logic chuyển đổi dữ liệu ảnh hưởng đến nội dung hiển thị.
- Xác định các guard, permission check và điều kiện authentication có liên quan.
- Phân biệt dependency cần kiểm tra với dependency không liên quan đến mục tiêu UI.
- Ghi lại kết quả phân tích, dependency graph và các giả định chưa được xác minh để những Skills tiếp theo có thể sử dụng.

Không chỉ dựa vào danh sách import để kết luận đã tìm thấy toàn bộ UI liên quan. Phải kiểm tra template, TypeScript và các cơ chế khởi tạo component động.

Không được tuyên bố đã bao phủ đầy đủ component hoặc dialog nếu chưa có đủ bằng chứng từ source code hoặc quá trình chạy thực tế.

### B. Sinh mock data

Mock data phải được suy ra từ model, interface, API contract và logic render thực tế trong repository.

Yêu cầu:

- Đúng cấu trúc dữ liệu và kiểu TypeScript.
- Đáp ứng các trạng thái UI cần thiết theo solution.
- Bao gồm dữ liệu có nội dung, trạng thái rỗng, loading, error và các trạng thái khác khi phù hợp.
- Có dữ liệu biên ảnh hưởng đến layout, ví dụ text dài, bảng nhiều dòng, field tùy chọn hoặc dữ liệu thiếu.
- Kích hoạt được những nhánh UI và child component cần kiểm tra.
- Mock API theo cơ chế hiện có của Playwright và repository.
- Tránh phụ thuộc backend thật, trừ khi solution yêu cầu rõ ràng.
- Dữ liệu phải deterministic, không phụ thuộc ngẫu nhiên hoặc thời gian thực khiến screenshot thay đổi giữa các lần chạy.
- Không tự ý thêm thuộc tính không tồn tại, vô hiệu hóa type checking hoặc tạo dữ liệu tùy tiện chỉ để test chạy qua.
- Không sửa business logic của ứng dụng chỉ để thuận tiện cho việc kiểm thử.

Nếu không xác định được API contract, model hoặc dữ liệu cần thiết, hãy báo cáo chính xác phần còn thiếu thay vì tự suy đoán.

### C. Sinh Playwright E2E Test

Test được sinh ra phải tuân theo convention và hạ tầng Playwright hiện có.

Yêu cầu:

- Tái sử dụng fixtures, helpers, authentication và mock utilities hiện có.
- Sử dụng locator ổn định, có ý nghĩa và assertion phù hợp.
- Điều hướng đến đúng route với các điều kiện cần thiết.
- Kiểm tra các trạng thái UI liên quan thay vì chỉ xác nhận trang ban đầu được mở.
- Mở dialog thông qua trigger thực tế trên UI và xác minh nội dung dialog đã render.
- Chuyển tab, mở dropdown, menu và thực hiện các tương tác cần thiết.
- Đảm bảo mỗi screenshot được chụp trong đúng trạng thái cần kiểm tra.
- Tránh hard-code selector dễ vỡ, dùng `waitForTimeout` tùy tiện, lặp test không cần thiết hoặc kiểm thử business flow ngoài phạm vi.
- Đảm bảo test có tính độc lập và kết quả có thể tái lập.
- Không vô hiệu hóa guard hoặc permission toàn cục để làm cho màn hình truy cập được.
- Không bỏ qua authorization phía backend.
- Khi cần quyền hoặc dữ liệu đặc biệt, sử dụng test configuration, fixture hoặc test utilities đã được phê duyệt.

Chỉ sinh các test cần thiết để đáp ứng mục tiêu UI coverage của solution. Không tự mở rộng thành bộ business E2E test toàn diện nếu solution không yêu cầu.

### D. Điều phối trạng thái UI và chụp screenshot

Tuân thủ chính xác chiến lược screenshot được mô tả trong file solution.

Với mỗi UI state cần kiểm tra:

- Thiết lập đúng route, viewport, dữ liệu test và trạng thái tương tác.
- Chờ ứng dụng và các element liên quan render ổn định.
- Chụp screenshot theo đúng cơ chế, tên file và vị trí lưu quy định trong solution.
- Đặt tên screenshot có thể truy ngược về route, component và UI state.
- Đảm bảo dialog và component có điều kiện được chụp khi thực sự hiển thị.
- Tránh chụp loading placeholder, animation đang chạy hoặc UI render chưa hoàn tất, trừ khi chính trạng thái đó cần được kiểm thử.
- Phân biệt rõ việc tạo screenshot, so sánh screenshot và phê duyệt baseline.

Không mặc định rằng chụp screenshot đồng nghĩa với giao diện đúng. Chỉ triển khai các cơ chế so sánh, baseline hoặc AI-assisted analysis theo đúng phạm vi và yêu cầu của solution.

### E. Validation và báo cáo

Sau khi sinh test:

- Kiểm tra import, TypeScript type, locator, mock response và đường dẫn screenshot khi có thể.
- Chạy test nếu môi trường cho phép.
- Báo cáo các component, dialog và UI state đã được kiểm tra.
- Phân biệt test đã sinh nhưng chưa chạy với test đã chạy thành công.
- Báo cáo test thất bại, dependency chưa giải quyết, thiếu mock data và UI state không thể truy cập.
- Phân biệt phạm vi đã được xác minh với phạm vi chỉ được suy luận từ source code.
- Không tuyên bố đã kiểm tra toàn bộ component chỉ vì route chính load thành công.

## 5. Thiết kế cơ chế phối hợp giữa các Skills

Xác định nên sử dụng một Skill điều phối chính cùng các Skills hỗ trợ hay một cấu trúc khác phù hợp hơn với repository.

Nếu sử dụng nhiều Skills, phải quy định rõ trách nhiệm, input, output và cách bàn giao kết quả giữa chúng.

Các nhóm trách nhiệm có thể bao gồm:

- Phân tích màn hình và dependency.
- Chuẩn bị mock data và mock API.
- Sinh Playwright E2E Test.
- Bao phủ UI state và chụp screenshot.
- Validation và báo cáo.

Đây là các ví dụ về trách nhiệm, không phải danh sách tên Skill bắt buộc. Chỉ tạo số lượng Skills thực sự cần thiết.

Skill đầu vào phải nhận path màn hình và điều phối workflow cần thiết. Người dùng không phải tự gọi từng Skill hỗ trợ hoặc cung cấp lại thông tin đã được phân tích.

Nếu cần truyền kết quả giữa các Skills, hãy sử dụng artifact hoặc cấu trúc dữ liệu trung gian phù hợp để tránh phân tích lặp lại và giữ nhất quán giữa dependency analysis, mock data, test và screenshot.

## 6. Quy chuẩn tạo Kiro Skills

Trước khi tạo file, kiểm tra phiên bản Kiro và convention Skills mà repository đang sử dụng.

- Tuân thủ đúng định dạng Skill, metadata, cấu trúc thư mục và quy tắc `SKILL.md` được môi trường hiện tại hỗ trợ.
- Tạo reference files hoặc scripts hỗ trợ khi thực sự cần thiết.
- Đặt các hướng dẫn dùng chung ở một nơi có thể tái sử dụng, tránh sao chép toàn bộ nội dung giữa các Skills.
- Tận dụng coding conventions và test conventions hiện có.
- Mô tả Skill rõ ràng để Kiro có thể chọn hoặc gọi đúng năng lực.
- Không hard-code đường dẫn, route, tên component hoặc mock data của riêng một màn hình vào hướng dẫn dùng chung.
- Không thiết kế Skill chỉ chạy cho một feature cụ thể.
- Không tạo thêm agent, steering system hoặc prompt framework nếu cấu trúc hiện tại không yêu cầu.

## 7. Phạm vi và các giới hạn

Nhiệm vụ hiện tại là **phân tích solution và tạo bộ Kiro Skills**, không phải triển khai toàn bộ hệ thống E2E và cũng không phải sinh test cho một màn hình cụ thể.

Không được:

- Sửa business logic của ứng dụng.
- Refactor component production.
- Thay đổi hành vi authorization.
- Vô hiệu hóa guard hoặc permission toàn cục.
- Sinh test cho toàn bộ route của ứng dụng trong nhiệm vụ tạo Skills này.
- Tạo mock API hoặc test infrastructure mới mà chưa kiểm tra cơ chế hiện có.
- Tự ý thay đổi chiến lược screenshot hoặc baseline của solution.
- Khẳng định test đã chạy thành công khi chưa thực sự thực thi.

Được phép tạo hoặc cập nhật các file Skill và tài liệu hỗ trợ trực tiếp phục vụ nhiệm vụ. Không thực hiện các thay đổi không liên quan trong repository.

## 8. Deliverables bắt buộc

Sau khi hoàn thành, cung cấp:

1. Tóm tắt phân tích file solution và các convention quan trọng của repository.
2. Kiến trúc bộ Skills được lựa chọn, trách nhiệm của từng Skill và cách chúng phối hợp.
3. Toàn bộ file Skills hoàn chỉnh, sẵn sàng sử dụng trong repository.
4. Các reference files hoặc scripts hỗ trợ nếu cần.
5. Ví dụ sử dụng Skill đầu vào với path của một màn hình.
6. Bảng mapping giữa yêu cầu trong solution và Skill chịu trách nhiệm thực hiện.
7. Báo cáo validation và các giới hạn chưa thể giải quyết do thiếu thông tin hoặc trở ngại kỹ thuật.

## 9. Tiêu chí nghiệm thu

Bộ Skills chỉ được xem là đạt yêu cầu khi:

- Đã phân tích toàn bộ solution và giữ nguyên các yêu cầu cốt lõi.
- Người dùng có thể cung cấp path source code của màn hình làm input chính.
- Workflow truy vết các component và dialog liên quan, không chỉ phân tích entry component.
- Mock data được suy ra từ kiểu dữ liệu, API contract và logic render thực tế.
- Playwright test có thể kiểm tra các UI state cần thiết và chụp screenshot theo solution.
- Tuân thủ convention Kiro, Angular và Playwright hiện có.
- Các Skills có trách nhiệm rõ ràng, không trùng lặp không cần thiết.
- Các giới hạn và phần chưa xác minh được báo cáo trung thực.
- Bộ Skills có thể tái sử dụng cho nhiều màn hình khác nhau mà không cần sửa hướng dẫn cốt lõi cho từng màn hình.

## 10. Cách thực hiện

Thực hiện theo thứ tự:

1. Đọc đầy đủ file solution.
2. Kiểm tra repository và các quy ước liên quan.
3. Xác định kiến trúc Skills phù hợp.
4. Tạo đầy đủ các file Skills và tài liệu hỗ trợ.
5. Kiểm tra cấu trúc, tính nhất quán và khả năng sử dụng của bộ Skills.
6. Báo cáo kết quả, cách gọi Skill đầu vào và các giới hạn còn lại.

Không dừng lại ở việc đề xuất kiến trúc. Phải hoàn thành các file Skills trong phạm vi được phép.

Nếu không tìm thấy file solution, yêu cầu cốt lõi bị mơ hồ hoặc có trở ngại kỹ thuật khiến không thể triển khai đúng, hãy nêu chính xác vấn đề và dừng trước khi tạo các Skills dựa trên giả định chưa được xác minh.
