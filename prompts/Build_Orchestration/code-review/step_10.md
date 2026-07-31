Final validation report đã kết luận `READY` và knowledge-base migration plan đã được user phê duyệt.

Hãy thực hiện knowledge-base cutover từ Amazon Q sang Kiro đúng theo kế hoạch đã duyệt.

## Mục tiêu

1. `.kiro/steering/angular-skills/` trở thành source of truth duy nhất về Angular coding convention.
2. Không còn active agent, skill hoặc prompt Kiro phụ thuộc vào `.amazonq/rules/coding_conventions/`.
3. Bảo toàn conventions và prompt review Amazon Q cũ trong archive để đối chiếu và migration.
4. Không để archive bị Kiro tự động nạp vào runtime context.
5. Không thay đổi nội dung hoặc hành vi review trong bước này.

## Điều kiện bắt đầu

Trước khi thay đổi:

1. Xác nhận final validation report có trạng thái `READY`.
2. Xác nhận migration plan đã được user phê duyệt.
3. Kiểm tra working tree.
4. Liệt kê những thay đổi hiện có không thuộc migration.
5. Nếu có thay đổi chồng chéo hoặc không xác định, dừng và báo lại.
6. Liệt kê chính xác:
   - File sẽ cập nhật reference.
   - File/folder sẽ được archive.
   - Archive path đã được phê duyệt.
   - Artifact sẽ được giữ nguyên.

## Source of truth

Sau cutover:

```text
.kiro/steering/angular-skills/
```

là source of truth duy nhất về Angular coding convention.

Không copy conventions trở lại agent, skill hoặc prompt.

## Cập nhật active dependencies

Đối với các dependency được migration plan phân loại là `ACTIVE`:

1. Chuyển reference từ `.amazonq/rules/coding_conventions/` sang steering phù hợp.
2. Sử dụng đúng `.kiro/steering/angular-skills/`.
3. Không nạp đồng thời hai knowledge base.
4. Không thay đổi workflow hoặc hành vi ngoài việc cập nhật knowledge source.
5. Không dùng glob quá rộng khiến toàn bộ steering được nạp không cần thiết.
6. Giữ nguyên cơ chế chọn convention theo loại file/task nếu đã tồn tại.

## Archive legacy artifacts

Đối với artifact được phân loại là `LEGACY`:

1. Di chuyển tới archive path đã được user phê duyệt.
2. Archive phải nằm ngoài:
   - `.amazonq/`
   - `.kiro/steering/`
   - `.kiro/skills/`
   - `.kiro/agents/`
   - `.kiro/prompts/`
   - Mọi resource glob có thể tự động nạp.

3. Giữ nguyên nội dung để đối chiếu.
4. Không chỉnh sửa conventions cũ trong archive.
5. Không xóa artifact.
6. Bảo toàn prompt review Amazon Q cũ để phân tích ở phase tiếp theo.
7. Nếu archive path có thể bị agent resource glob match, dừng và đề xuất path khác.

## Không được thực hiện

- Không sửa production source.
- Không thay đổi nội dung `steering/angular-skills`.
- Không tối ưu hoặc rewrite prompt review Amazon Q.
- Không tạo review agent mới.
- Không tạo review skill.
- Không thay đổi scoring hoặc threshold.
- Không xóa `.amazonq` ngoài đúng artifact đã được phê duyệt.
- Không commit hoặc push nếu user chưa yêu cầu.

## Validation sau cutover

### 1. Reference validation

Tìm lại mọi reference tới:

```text
.amazonq/rules/coding_conventions
```

Phân loại kết quả còn lại:

- Archive-only reference.
- Legacy documentation.
- Active runtime dependency.
- Unknown.

Không được còn `Active runtime dependency`.

### 2. Runtime validation

Kiểm tra:

1. Active Kiro agents chỉ sử dụng steering mới.
2. Active skills không reference conventions cũ.
3. Active prompts không yêu cầu nạp cả hai knowledge base.
4. Archive không nằm trong resource glob.
5. Default review hoặc implementation workflow không bị mất knowledge source.

### 3. Integrity validation

Kiểm tra:

1. `steering/angular-skills` không bị thay đổi.
2. Nội dung archived conventions giữ nguyên.
3. Prompt review Amazon Q được bảo toàn nguyên trạng.
4. Không có source file bị thay đổi.
5. Không có artifact ngoài phạm vi bị di chuyển.

## Kết quả đầu ra

Cung cấp:

1. Danh sách file đã cập nhật reference.
2. Danh sách artifact đã archive.
3. Archive path.
4. Diff đầy đủ để user review.
5. Reference scan trước và sau.
6. Active dependency matrix sau migration.
7. Xác nhận:
   - Không còn active runtime dependency vào conventions cũ.
   - Steering mới không bị thay đổi.
   - Prompt review Amazon Q cũ vẫn được bảo toàn.
   - Archive không được tự động nạp.
   - Source code không bị thay đổi.

8. Chờ user xác nhận; không tự phân tích hoặc migrate prompt review.
