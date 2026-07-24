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
After collecting all raw diffs, transform EACH file's diff into the compact review format before dispatching to agents. This reduces token usage by -50% while preserving informat: needed for review.
#*Transformation rules (apply in THIS order):**
1. FIRST, for each * block, identify the enclosing scope (class/method/function name) from the nearest context line above it - capture it for the '#** Scopes' header BEFORE remor anything.
2. KEEP all '+' Lines (newladded ondaha