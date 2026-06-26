默认流程为
0. 创建 YYYYMMDD_HHMM_review 项目文件夹
1. deep-research 做选题与研究定位
2. nature-academic-search 做正式文献检索和 EndNote 文献库
3. academic-paper 做架构设计，用户选择模式并确认架构
4. academic-paper 写初稿
5. 设计机制图科学内容
6. imagegen + Python 绘制投稿级机制图
7. nature-reviewer 做第一轮高标准预审
8. nature-response 整理第一轮意见，再用 academic-paper revision 修改
9. academic-paper-reviewer 做第二轮综合盲审
10. academic-paper revision 做第二轮修改
11. citation-check 和内部规则做完整性检查
12. 推荐 3-5 个目标期刊
13. 用户选择期刊后解析 Guide for Authors
14. 按目标期刊格式生成 EndNote-ready Word
15. 按目标期刊要求整理最终图片
16. 写 cover letter
17. 整理最终投稿材料
# Review Writing Mechanism Figures

`review-writing-mechanism-figures` is a Codex skill for biomedical review projects that need a complete, submission-oriented workflow: topic selection, literature search, outline confirmation, manuscript drafting, mechanism-figure generation, two-round review and revision, target-journal formatting, EndNote-ready Word output, cover letter drafting, and organized submission packaging.

It is designed especially for medical and life-science reviews, including oncology, immunology, metabolism, single-cell/spatial omics, and mechanism-focused translational topics.

## What This Skill Does

- Creates a timestamped review project folder.
- Guides topic selection and review-positioning.
- Builds a verified literature pool and EndNote-importable reference library.
- Requires the user to choose the reference count.
- Requires the user to confirm the manuscript outline before drafting.
- Uses `imagegen` for biological mechanism-figure base images.
- Uses Python post-processing for final figure labels, font control, leader lines, and overlap avoidance.
- Runs first-round high-standard review with `nature-reviewer`.
- Runs first-round response planning with `nature-response`.
- Applies first-round revision with `academic-paper revision`.
- Runs second-round broad pre-submission review with `academic-paper-reviewer`.
- Applies second-round revision with `academic-paper revision`.
- Performs final integrity and citation checks.
- Recommends 3-5 target journals.
- Formats the final manuscript only after the user selects the target journal.
- Produces an EndNote-ready Word manuscript.
- Drafts a target-journal-specific cover letter.
- Organizes final and revision files into a clean submission package.

## Key Design Rules

This skill is an orchestrator. It does not duplicate the full logic of upstream skills. Instead, it dispatches specialized skills at the correct stage:

- `deep-research` for topic positioning.
- `nature-academic-search` for literature search and citation verification.
- `academic-paper` for writing, revision, citation checking, and formatting.
- `nature-reviewer` for first-round high-standard review.
- `nature-response` for first-round response strategy.
- `academic-paper-reviewer` for second-round broad review.
- `imagegen` for biological figure base images.

Important hard rules:

- Do not prescribe the reference count. Ask the user.
- Explain mode advantages and disadvantages before every mode choice.
- Recommend a mode with reasons, then let the user choose.
- Do not draft before the user confirms the outline.
- Do not draw final figures before the user confirms the scientific content.
- Do not rely on imagegen to produce final text labels.
- First-round revision must run `nature-response` before `academic-paper revision`.
- Final formatting must wait until the user selects a target journal.
- Word references must use an EndNote-ready workflow.
- A target-journal-specific cover letter is required.

## Default Workflow

1. Create a project folder named `YYYYMMDD_HHMM_review`.
2. Run topic selection with `deep-research`.
3. Run literature search and reference-pool creation with `nature-academic-search`.
4. Design two manuscript outlines with `academic-paper`; user confirms one.
5. Draft the manuscript with `academic-paper`.
6. Design mechanism figures.
7. Generate mechanism figures with `imagegen` plus Python labeling.
8. Run first-round review with `nature-reviewer`.
9. Use `nature-response` to convert review reports into a revision strategy.
10. Use `academic-paper revision` to apply the first revision.
11. Run second-round review with `academic-paper-reviewer`.
12. Use `academic-paper revision` to apply the second revision.
13. Run final citation and integrity checks.
14. Recommend target journals.
15. Parse the selected journal's Guide for Authors.
16. Format the final manuscript and figures according to the selected journal.
17. Generate an EndNote-ready Word manuscript.
18. Draft a cover letter.
19. Assemble the final submission package.

## Project Folder Layout

Each project starts with this layout:

```text
YYYYMMDD_HHMM_review/
  final_submission/
    figures/
    tables/
  revision/
    original/
    round1_nature_review/
    round2_academic_review/
    figures_working/
    literature/
  journal_selection/
  process_record/
```

Final files go into `final_submission/`. Original drafts, review reports, response documents, revision drafts, and working figures go into `revision/`. Journal selection materials go into `journal_selection/`. Workflow logs and integrity checks go into `process_record/`.

## EndNote And Word Automation

This skill includes a practical EndNote automation boundary in:

```text
references/endnote-word-automation.md
```

Local testing showed:

- Word COM automation is available.
- Word 16.0 is installed.
- `EndNote Cite While You Write` is loaded in Word.
- `ENFormatBibliography` can be invoked programmatically.
- Direct citation insertion was not reliably available through automation.

Therefore, the skill promises:

- EndNote-importable reference library files such as `.ris` or `.nbib`.
- EndNote-ready Word manuscripts.
- Best-effort automatic bibliography formatting.
- Clear manual EndNote Cite While You Write steps when automation fails.

It does not promise guaranteed fully automatic EndNote citation insertion.

## Installation

Copy the skill folder into your Codex skills directory:

```text
%USERPROFILE%\.codex\skills\review-writing-mechanism-figures
```

The installed folder should contain:

```text
review-writing-mechanism-figures/
  SKILL.md
  agents/
    openai.yaml
  references/
    endnote-word-automation.md
  scripts/
```

Restart Codex or reload skills if needed.

## Example Prompt

```text
[$review-writing-mechanism-figures]
启动综述书写与机制图绘制流程。
主题：肝细胞癌免疫治疗耐受与空间免疫代谢生态位。
目标：中科院二区综述。
要求：先做方向和文献检索，再设计架构，确认后写作，绘制3-4张机制图，进行两轮盲审修改，最后根据目标期刊格式生成EndNote-ready Word、最终图片和cover letter。
```

## Repository Contents

- `SKILL.md`: Main orchestration workflow and hard rules.
- `agents/openai.yaml`: UI metadata for Codex skill listing.
- `references/endnote-word-automation.md`: Word + EndNote automation notes and fallback procedure.
- `scripts/`: Reserved for future deterministic helper scripts.

## Notes

This skill is intended for serious review-writing projects rather than quick one-off drafting. It prioritizes reproducibility, version control, target-journal compliance, figure quality, and submission readiness.
