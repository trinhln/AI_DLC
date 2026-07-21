Analyze the existing `create-standalone-module` prompt and convert it into a well-structured Kiro Agent Skill for the same task.

Do not copy the prompt mechanically. Analyze its workflow, rules, templates, examples, and reusable knowledge, then organize them appropriately into `SKILL.md` and supporting `references/`, templates, or scripts when needed.

Follow Kiro Agent Skills best practices with progressive disclosure:

- Keep `SKILL.md` concise and focused on workflow.
- Move detailed templates/examples into supporting resources.
- Remove duplicated, obsolete, or unnecessary instructions.
- Preserve all important behavior and requirements of the original prompt.

This is a completely new project. Base the conversion only on the current project and the existing prompt. Do not reuse conventions or assumptions from other projects.

After conversion, validate the skill structure and explain briefly how the new skill replaces the old prompt.

===================================

Sau khi convert thì cần thực hiện test.

###### Test 1 — Gọi Skill trực tiếp

```
/create-standalone-module
Create a standalone module named user-management.
```

###### Test 2 — Test auto-activation

Nếu Test 1 đạt, mở một session/context mới rồi không gọi tên Skill:
=> Mục tiêu là xem Kiro có tự discover create-standalone-module dựa trên name + description của SKILL.md hay không.
Ta cần phân biệt kết quả:

```
Create a new standalone module named product-management
following the project's standard module structure and templates.
```

Note:

```
Test 1 PASS + Test 2 PASS
→ Skill tốt và auto-activation tốt.

Test 1 PASS + Test 2 FAIL
→ Nội dung Skill tốt nhưng description/discovery chưa tốt.
→ Cần chỉnh frontmatter của SKILL.md.

Test 1 FAIL
→ Bản thân workflow/template của Skill cần sửa trước.

```
