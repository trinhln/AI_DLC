```

Hãy phân tích cấu hình Kiro hiện tại của project và tạo một workspace custom agent có tên `nbe-code-implementation`, chuyên dùng cho mọi tác vụ tạo mới, chỉnh sửa, fine-tune, sửa bug và refactor code.

## Mục tiêu

Agent phải giúp code được implement đúng theo coding convention hiện có trong:

- `.kiro/steering/`
- `.kiro/steering/angular-skills/`

Không chuyển đổi, sao chép hoặc chỉnh sửa bộ steering hiện tại. Agent phải chủ động xác định và đọc đúng convention cần thiết trước khi viết code, kể cả khi file đích chưa tồn tại.

## Trước khi tạo agent

1. Kiểm tra cấu trúc `.kiro` hiện tại.
2. Đọc các custom agent đang có để sử dụng đúng schema, vị trí file, tool names và convention của project.
3. Xác định chính xác custom agent hiện có đang thực hiện review và chấm điểm code trên thang điểm 100.
4. Kiểm tra cách gọi custom agent review đó dưới vai trò sub-agent.
5. Kiểm tra cách `inclusion: fileMatch` và `fileMatchPattern` đang được cấu hình trong `steering/angular-skills`.
6. Xác định cách cho custom agent truy cập các steering cần thiết mà không nạp toàn bộ `angular-skills` vào context cho mọi task.
7. Không giả định schema, agent name hoặc tool name; phải dựa trên cấu hình Kiro hiện có trong project.

## Workflow bắt buộc của agent

### Phase 1 — Understand

Trước khi sửa code, agent phải:

1. Hiểu yêu cầu và phạm vi thay đổi.
2. Phân loại tác vụ: component, template, form, service, API, model, routing, unit test, bug fix, refactor hoặc loại khác.
3. Xác định các file dự kiến tạo mới hoặc chỉnh sửa.
4. Nếu thiếu thông tin quan trọng có thể làm thay đổi solution, phải hỏi lại user trước khi implement.

### Phase 2 — Load conventions

1. Dựa trên loại tác vụ và đường dẫn dự kiến của các file đích, xác định các steering phù hợp.
2. Kiểm tra `fileMatchPattern` của từng steering liên quan.
3. Chủ động đọc các convention phù hợp trước khi viết code.
4. Không chỉ phụ thuộc vào việc file đích đã tồn tại, vì agent phải hỗ trợ cả việc tạo file mới.
5. Chỉ đọc những steering cần thiết cho task để hạn chế token.
6. Core steering và các rule có mức `MUST`/`MUST NOT` phải được ưu tiên.

### Phase 3 — Inspect implementation context

1. Steering là source of truth duy nhất về coding convention và implementation pattern.
2. Chỉ đọc những source file trực tiếp liên quan đến yêu cầu để:
   - Hiểu behavior hiện tại.
   - Xác định dependency và data flow.
   - Xác định vị trí cần thay đổi.
   - Tránh làm ảnh hưởng logic hiện hữu.
3. Không scan toàn bộ project để tìm implementation tương tự hoặc tự suy luận convention từ source code.
4. Không coi code hiện hữu là coding convention, vì project có thể còn chứa code legacy.
5. Chỉ đọc template hoặc reference implementation khi:
   - Steering chỉ định rõ file cần tham khảo; hoặc
   - User chủ động cung cấp file mẫu.
6. Nếu source code hiện tại xung đột với steering:
   - Code mới hoặc phần code được chỉnh sửa phải tuân theo steering.
   - Không tự refactor phần code ngoài phạm vi task.
   - Báo rõ xung đột nếu nó ảnh hưởng đến implementation.

### Phase 4 — Implement

1. Tuân thủ toàn bộ convention đã được nạp.
2. Chỉ thay đổi trong phạm vi task.
3. Không tự ý refactor code không liên quan.
4. Không tự thêm hoặc nâng cấp dependency.
5. Không sử dụng Angular API không được hỗ trợ bởi version của project.
6. Giữ nguyên behavior hiện tại, trừ phần được yêu cầu thay đổi.
7. Ưu tiên chỉnh sửa tối thiểu nhưng hoàn chỉnh và nhất quán.
8. Không tự động tạo hoặc cập nhật unit test nếu user không yêu cầu rõ ràng.

### Phase 5 — Review-code quality gate

Sau khi implement, agent phải sử dụng custom agent `review-code` hiện có của project để review toàn bộ code đã thay đổi.

1. Cung cấp cho review agent:
   - Yêu cầu ban đầu của user.
   - Phạm vi implementation.
   - Danh sách file đã tạo hoặc chỉnh sửa.
   - Toàn bộ diff cần review.
   - Các steering/convention chính đã được áp dụng.

2. Không tự thay đổi tiêu chí hoặc cơ chế chấm điểm của review agent.
3. Review agent chấm code trên thang điểm 100.
4. Nếu điểm số từ 85 trở lên:
   - Quality gate được thông qua.
   - Chuyển sang Phase 6.

5. Nếu điểm số dưới 85:
   - Quality gate thất bại.
   - Đọc đầy đủ các lỗi và đề xuất của review agent.
   - Sửa những lỗi thuộc phạm vi task.
   - Không sửa máy móc những đề xuất không phù hợp với yêu cầu hoặc convention.
   - Gọi lại review agent sau khi cập nhật.

6. Cho phép tối đa 3 vòng review.
7. Nếu sau 3 vòng điểm vẫn dưới 85:
   - Dừng vòng lặp.
   - Không tuyên bố task hoàn thành.
   - Báo cáo điểm số cuối cùng, các lỗi còn lại và nguyên nhân chưa thể xử lý.

8. Không bỏ qua quality gate nếu review agent có thể hoạt động.
9. Nếu không thể gọi review agent, phải báo rõ lý do và không được tự coi quality gate là đã thông qua.

### Phase 6 — Validation

Chạy các validation phù hợp và có sẵn trong project:

- Formatter.
- Lint.
- Type checking hoặc build phù hợp với phạm vi thay đổi.

Ưu tiên validation theo phạm vi thay đổi và tránh chạy các command tốn thời gian không cần thiết.

Không tự động tạo, cập nhật hoặc chạy unit test nếu user không yêu cầu rõ ràng. Unit test được xử lý trong task riêng khi code chuẩn bị lên UAT hoặc khi user chủ động yêu cầu.

Không được tuyên bố validation thành công nếu chưa chạy kiểm tra. Nếu không thể chạy, phải nói rõ kiểm tra nào chưa chạy và lý do.

## Kết quả cuối cùng của mỗi task

Agent phải báo cáo ngắn gọn:

- Những file đã thay đổi.
- Các steering/convention chính đã áp dụng.
- Canonical example đã tham khảo.
- Điểm số cuối cùng từ review agent.
- Số vòng review đã thực hiện.
- Validation đã chạy và kết quả.
- Những kiểm tra chưa chạy.
- Những giả định hoặc rủi ro còn lại.

## Phạm vi sử dụng

Sử dụng agent cho:

- Tạo file/code mới.
- Chỉnh sửa hoặc fine-tune code.
- Sửa bug có thay đổi source.
- Refactor.
- Tích hợp API.
- Viết hoặc cập nhật unit test khi user yêu cầu rõ ràng.

Agent không thay thế custom agent review hiện có. Agent phải sử dụng review agent đó làm quality gate sau khi implement.

## Yêu cầu khi tạo

1. Tạo agent tại đúng workspace location theo cấu trúc project hiện tại.
2. Cấp đúng các tool cần thiết để đọc, tìm kiếm, chỉnh sửa source, chạy validation và gọi custom review agent.
3. Nếu Kiro yêu cầu tool `subagent` để gọi review agent, phải thêm tool này vào cấu hình.
4. Không cấp tool không cần thiết.
5. Không tạo thêm custom sub-agent mới.
6. Chỉ orchestration với review agent hiện có tại Phase 5.
7. Không sửa steering, skills, tiêu chí chấm điểm hoặc các agent hiện có.
8. Không duplicate toàn bộ convention vào prompt của agent.
9. Prompt agent chỉ điều phối workflow; steering hiện tại tiếp tục là source of truth.
10. Thêm description rõ ràng để developer hiểu agent dùng cho mọi tác vụ implement code.
11. Validate cấu hình agent bằng cơ chế/schema validation mà phiên bản Kiro hiện tại hỗ trợ.

Sau khi hoàn thành, hãy cung cấp:

1. Danh sách file đã tạo.
2. Nội dung cấu hình agent.
3. Giải thích cách agent lựa chọn steering phù hợp cho file mới và file hiện hữu.
4. Giải thích cách agent gọi `review-code` và xử lý quality gate 85 điểm.
5. Hướng dẫn ngắn để developer chọn và sử dụng agent.
6. Một kịch bản test nhỏ, an toàn, không ảnh hưởng production code để kiểm tra toàn bộ quy trình:
   - Load convention.
   - Implement.
   - Gọi review agent.
   - Xử lý kết quả chấm điểm.
   - Chạy validation.

```
