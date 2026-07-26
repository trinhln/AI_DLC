# Review Pull Request (Multi-Agent)
Review Angular PRs from Bitbucket using a multi-agent architecture.
#9 Usage
use •kiro/prompts/review-pr.nd and review PR:<PR_LINK-
**Example:**
use •kiro/prompts/review-pr.nd and review PR: ${link)
-
# Process (Orchestration)
### Step 1: Fetch COMPLETE PR Diff
1. Parse PR Link - extract "project", •repository, 'prid
2. Fetch PR info: "get_pull_request(project, repository, prid)®
3. Fetch FULL diff using "get diff_paginated®:
- Use 'filesPerPage=5° to reduce total pages.
- Use contextLines=3' so a few unchanged lines around each * block are preserved (needed for cross-file/context reasoning - see Step 1.5)
- Start with 'startPage=1*
- Check 'pagination-hasNextPage in response
- If true, fetch next page with startPage=2*, then '3', etc.
- **CRITICAL: Continue until 'hasNextPage=false*** - do NOT stop at page 1
- **NEVER use PR summary as substitute for unfetched pages**
- **Safety cap:* the tool defaults to maxPages-50". With °f1lesPerPage=S that is ~250 files. If the cap is reached and "hasNextPage" is still 'true", STOP and warn the users ' too large (>250 files) - reviewed first N files only." Do not silently drop files.
- Collect ALL file diffs across all pages
4. **Transform diff - optimized format** (Step 1.5):
After collecting all raw diffs, transform EACH file's diff into the compact review format before dispatching to agents. This reduces token usage by -50% while preserving informat: needed for review
*Transformation rules (apply in THIS order):**
1. FIRST, for each * block, identify the enclosing scope (class/method/function name) from the nearest context line above it - capture it for the '# Scope:' header BEFORE removing anything•
2. KEEP all ** lines (new/added code - the review target)
3. KEEP up to 3 context lines immediately adjacent to each '*' block (unchanged code). Rationale: rules like [03) Memory Management and the meta-rule "Code Already Correct" need to see whether e.g. 'DestroyRef is already injected nearby. Mark these clearly (no prefix) so agents know they are context, not new code.
4. REMOVE all '- lines (deleted code - not reviewable)
5. REMOVE context lines that are NOT adjacent to any *+* block (far-away unchanged code)
6. KEEP file path from 'diff —git' header
7. KEEP line numbers from "@@* hunk headers
> **Note on file type:** the *### Scope:' header is meaningful for 'ts' files (class/method). For •component.html files there is no class/method - set '## Scopes template' (or the nearest structural element such as a '*ngFor*/'*ngIf block) and focus the review on the ' lines.
**Output format per file:**
*## File: <full/path/to/file.ts>
### Scope: «ClassName or methodName containing the code>
### Lines: ‹start>-<end>
«added code without + prefix>
### Lines: «start>-<end>
### Scope: ‹next scope if different»
«next block of added code>

**Example - Before (raw diff):**
***diff
www.
diff -git a/src/app/features/param-rule/param-rule-create.component.ts
CO -1, 15 +1,45 ca
import { Component, OnInit } from '@angular/core': timport { inject, DestroyRef } from '@angular/core'; timport { takeUntilDestroyed } from '@angular/core/rxjs-interop'; import { ForGroup, FormControl, Validators } from '@angular/forms';
-import { ParamRuleService } from '•/param-rule.service'; timport { ParamuleService } from '../services/param-rule.service';
@Component ({
selector: 'app-param-rule-create', templateUrl: '•/param-rule-create.component.html',
+
providers: [ParamRuleService]
-export class ParamuleCreateComponent implements OnInit {
- constructor(private service: ParamRuleService) ()
+export class ParamuleCreateComponent implements OnInit {
+ private destroyRef = inject (DestroyRef);
+
private service = inject (ParamRuleService);
+
+
+
ngOnInit() {
this. loadData();
+ }
+
+.
+
+
+
+
private loadData() ‹ this.service.getData()
• pipe(takeUntilDestroyed (this.destroyRef))
• subscribe (res → {
this. formGroup-patchValue(res);
7) ;

**Example - After (optimized format sent to agent):**
#H File: src/app/features/param-rule/param-rule-create. component. ts
### Scope: imports
### Lines: 2-4
import { inject, DestroyRef } from '@angular/core': import { takeUntilDestroyed } from '@angular/core/rxjs-interop'; import { ParamuleService } from '••/services/param-rule service':
### Scope: @Component decorator
### Lines: 12
providers: [ParamRuleService]
### Scope: class ParamRuleCreateComponent
### Lines: 15-35
private destroyRef = inject(DestroyRef);
private service = inject (ParamRuleService);
ngUnInit() {
This. LoadData():
private loadData () {
this-service-getData()
- pipe(takeUntilDestroyed(this.destroyRef))
• subscribe(res →> {
this.formGroup-patchValue(res);
7):

**Why this works:**
- Agent sees ALL added code (nothing skipped)
- Agent knows exact file path and line numbers (can reference in findings)
- Agent knows scope/context (class name, method name) without needing full file
- A few adjacent context lines remain, so the agent can confirm "already correct" cases (e-g. DestroyRef already injected) instead of raising false positives
- ~50% fewer tokens than raw diff → large PRs fit in context window
### Step 2: Classify Files
Group files by type for agent routing:
| File Pattern | Agents to Invoke I
1-1
** component.ts* | review-angular-architecture, review-typescript-typing, review-correctness-security, review-quality-testing I
**service.ts' | review-angular-architecture, review-typescript-typing, review-correctness-security I
** interceptor.ts*, *guard.ts' | review-angular-architecture, review-typescript-typing |
*model. ts', '*enum.ts', "*.constant.ts" | review-typescript-typing I
** component-html* | review-correctness-security, review-quality-testing |
"*"spec.ts* | review-quality-testing |
| Binary assets in "src/assets/' (*-xlsx*, **-pdf", large images/templates) | review-correctness-security (rule (16) File Storage) |
"*module.ts', "*.scss" | Skip |
• **Note for rule [16] File Storage:** detect from the diff file headers (a NEW binary file
> added under src/assets/templates/' or similar). The file content itself is not reviewable
> as text - route the file PATH (from 'diff git' header) to 'review-correctness-security'
> so it can flag committed binary templates.
I
### Step 3: Dispatch to Subagents
Invoke the **4 types of specialized review subagent in parallel using the subagent tool. Each agent receives the optimized diff** (from Step 1.5) of files assigned to it (per Step 2 classification). Note: a single agent type MAY be invoked more than once (see Prompt construction rule #4) when its assigned diff is large - "in parallel" refers to the 4 agent «types*. not a fixed count of 4 invocations.
1. **review-angular-architecture** - Component structure, memory, services, error handling
2. **review-typescript-typing** - Variables, types, constants, naming, imports
3. **review-correctness-security** - Security, forms, hardcoded logic, file storage [16)
4. **review-quality-testing** - Performance, test coverage

##*# Context files to attach per agent:
When invoking each agent via 'subagent', include the relevant coding convention rule files in the prompt so the agent has direct access to the full rules:
| Agent | Context Files (from • amazonq/rules/coding_convention/) |
JI-I
i review-angular-architecture | *00-meta-rules.md*,
*02-component-structure.md*, '03-memory-management-md*, '04-service-architecture.md, "08-error-handling-md*, '15-dialog-patterns.md" |
I review-typescript-typing | "00-meta-rules.md*, '12-variable-declaration.md*,
"06-typescript.md,
"9-constants-enums.nd°, 01-file-naning-md, "87-1mport-organization.md°
I review-correctness-security | 00-meta-rules.d, '13-security.md', '05-form-handling-md', '11-code-quality.md', '16-file-storage.md° | I review-quality-testing | "00-meta-rules-nd*, "10-performance.md*, "14-test-coverage.md* |
> **Why attach '00-meta-rules-md to every agent:** meta-rules have the HIGHEST priority and define overrides (Warning-Only items, "Not Issues" that must PASS, context/deleted code to ignore). Attaching it lets each agent apply these overrides *at the source* - so it never raises a FAIL that Step 4 would just have to downgrade. This reduces false positives (e.g.
flagging
*displayedColumns*, empty 'finalize()") and keeps scoring consistent before results reach the orchestrator.
#### Prompt construction rules:
1. **Send optimized diff** - Send the transformed format from Step 1.5 (added * lines + a few adjacent context lines + file path + line numbers + scope). Do NOT send •- deleted lines or far-away context.
3. wer use lachdts = 0 No te 1 3 as a ae coey ony rend The gent cann revies wat it canot s00.
4. **Split by file if needed** - If total optimized diff for one agent exceeds ~2,000 lines, invoke the agent multiple times (once per file or group of files). Each file's diff must be
COMPLETE.
1e one per Tate or grue of fa), act Fers dit wet be
5. **Preserve metadata** - Each file section MUST include '### File:*, *### Scope:*, and *##* Lines:* headers exactly as produced in Step 1.5.
6. **Include instruction** - Tell agent to review the ** (new/added) lines in the optimized diff. Adjacent context lines (no prefix) are reference only - do NOT raise issues on them.
7. **Allow cross-file verification** - Tell the agent it MAY use "grep' and read tools on the workspace to verify findings that cannot be confirmed from the diff alone (e.g. whether a matching enum/const already exists in ** constant. ts'/'*-model, ts*/**.enum.ts', or whether openDialogRight' exists). The optimized diff is the review target, NOT the only data source.
8. **Pass the file manifest to 'review-quality-testing'** - This agent's rule [14] needs to know which files are NEW and whether a NEW file has a matching
'spects'. Include the full
List of changed files (with new/modified status from the diff headers) so it can detect a NEW '*service.ts /*. component. ts' that is missing its ' spec.ts'.
9. **Include expected output format**: file, line, category, severity, description, currentCode, fixedCode.
wienemissing tient est one team her see part tee carrected
~~~ **Architectural fix** (e.g., "component too large", "split into sub-components"): provide a concrete skeleton showing the extraction - new file name, extracted class/template with key logic moved, and updated parent referencing the new child.
..~ **Cannot provide fix** (e-g., depends on unknown business logic, needs team decision): downgrade severity to WARNING (0 pts) and explain why a fix cannot be determined from the diff
alone.
..~ **Comments are NOT fixes** - *// should use tw-bg-gray-2' inside currentCode does NOT count as fixedCode. The fixedCode block must be runnable/applicable code.

lab uttere
## Process (Orchestration)
### Step 3: Dispatch to Subagents #### Prompt construction rules:
### Step 4: Apply Meta-Rules
> Agents already receive "OD-meta-rules.md' (Step 3) and should apply these at the source. This step is a final safety net to catch anything that slipped through.
After collecting all agent results, apply overrides:
**Downgrade to WARNING (0 pts):**
-Enty constructor with tect
- Wrong property order
- Mixed import ORDER ° [07a)* - Warning (note: cherry-picking Lodash/date-fns ° [07b)* stays Major -10, NOT downgraded)
- Empty finalize()
**Mark as PASS (remove issue):**
- Display text (button labelse headingse Placeholders)
- Technical identifiers (displayedColumns, route paths, CSS classes)
- Code already correct (readonly present, DestroyRef injected)
**IGNORE:**
deleted
d code fines with.
- Issues on
context lines (no prefix)
### Step 5: Deduplicate
If multiple agents flag same file: line:
-Keep highest severity
- Remove duplicates