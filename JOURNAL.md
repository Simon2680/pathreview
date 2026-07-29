# PathReview Journal — Simon Iradukunda

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/147

**Issue title:** Resume section detection fails on text with leading whitespace

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The `ResumeParser` class in `ingestion/parsers/resume_parser.py` fails to detect sections when the text has leading whitespace. This is because the regular expressions in `_detect_sections()` are anchored to the start of the line or string (`^Experience`, `\nExperience`) without allowing for preceding space characters. In many parsed PDF documents, the extracted text maintains margins or lists with leading tabs/spaces, causing the section detection to miss important sections like Education or Skills. A successful fix will modify the regular expressions to allow optional whitespace after a newline or string start, ensuring sections are correctly identified.

**Branch name:** fix/147-resume-section-whitespace

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

**Selection notes / 'Is this right for me?' reasoning:**
1. **Localizability**: The bug is isolated entirely to the regex pattern matching in `ingestion/parsers/resume_parser.py`. It doesn't require modifying database models, APIs, or the React frontend.
2. **Reproduction**: The repository includes failing unit tests (`test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, `test_detect_sections`) which fail consistently and provide a clear, local feedback loop.
3. **No External Dependencies**: The bug can be fixed and verified completely offline without needing active GitHub tokens or LLM APIs.
4. **Scope Fit**: As a Tier 1 issue, it represents a well-defined task (updating regular expressions and markdown cleanup logic) that matches the scope of a starter contribution.

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Simon2680/pathreview/commit/24e5bab59fbd76dcd22ba67adbdf67fcc393f952

**Reproduction summary:**
I reproduced the issue locally by running pytest on `tests/unit/test_resume_parser.py` and creating a reproduction test `test_reproduce_issue_147_leading_whitespace` with space and tab indented section headers. I observed that `ResumeParser._detect_sections()` returned an empty list `[]` because its regex patterns (`^Header`, `\nHeader`) fail to match lines with leading whitespace.

**PLAN.md link:** https://github.com/Simon2680/pathreview/blob/fix/147-resume-section-whitespace/PLAN.md

**Walkthrough video (recommended):** N/A

**Blockers or open questions:**
None. The root cause in `ingestion/parsers/resume_parser.py` is fully understood and verified through failing test assertions.
