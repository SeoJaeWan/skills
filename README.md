> **Fork Notice — SeoJaeWan/skills**
>
> [anthropics/skills](https://github.com/anthropics/skills)의 개인 fork입니다.

> **eval 뷰어 `</script>` 이스케이프 수정**
>
> **목적:** eval 출력에 `</script>` 문자열이 포함될 경우(예: JSON-LD 수정 가이드), 브라우저 HTML 파서가 메인 `<script>` 블록을 조기 종료시켜 리포트가 깨지는 문제 수정.
>
> **원인:** `generate_review.py`에서 `json.dumps` 결과를 `<script>` 블록 안에 직접 삽입하는데, JSON 값 내부의 `</script>` 문자열을 HTML 파서가 script 종료 태그로 인식.
>
> **수정 범위:**
> - `skills/skill-creator/eval-viewer/generate_review.py` — JSON 임베딩 전 `</script>` → `<\/script>` 이스케이프 추가

> **한글 UI 적용 및 eval 탐색 버그 수정**
>
> **목적:** skill-creator 뷰어를 한글 기반으로 전환하고, 스크립트 전반의 UTF-8 인코딩 누락 수정 및 `eval_metadata.json`/`grading.json` 탐색 버그를 해결.
>
> **수정 범위:**
>
> *한글 UI 적용*
> - `skills/skill-creator/eval-viewer/viewer.html` — 뷰어 UI 텍스트 전체 한글화 (헤더, 탭, 버튼, 피드백, 벤치마크 등)
>
> *UTF-8 인코딩 수정 (스크립트 전반)*
> - `skills/skill-creator/scripts/generate_report.py` — 파일 I/O에 `encoding="utf-8"` 명시
> - `skills/skill-creator/scripts/improve_description.py` — 파일 I/O에 `encoding="utf-8"` 명시
> - `skills/skill-creator/scripts/quick_validate.py` — 파일 I/O에 `encoding="utf-8"` 명시
> - `skills/skill-creator/scripts/run_eval.py` — 파일 I/O에 `encoding="utf-8"` 명시
> - `skills/skill-creator/scripts/run_loop.py` — 파일 I/O에 `encoding="utf-8"` 명시
> - `skills/skill-creator/scripts/utils.py` — 파일 I/O에 `encoding="utf-8"` 명시
>
> *eval_metadata.json / grading.json 탐색 버그 수정*
> - `skills/skill-creator/eval-viewer/generate_review.py` — `build_run()`의 파일 탐색을 고정 depth(parent)에서 `run_dir` → `root` 상위 순회 방식(`_find_ancestor_file`)으로 변경. 디렉토리가 2단계 이상 중첩될 때(예: `eval-1/with_skill/run-1/outputs/`) `eval_metadata.json`과 `grading.json`을 찾지 못하던 문제 해결.

> **Windows UTF-8 인코딩 수정**
>
> **목적:** Windows 환경(cp949 등 비UTF-8 로케일)에서 발생하는 `UnicodeDecodeError`를 수정하기 위한 커스텀 fork.
>
> **수정 범위:**
> - `skills/skill-creator/eval-viewer/generate_review.py` — 파일 I/O에 `encoding="utf-8"` 명시 (9개 지점)
> - `skills/skill-creator/scripts/aggregate_benchmark.py` — 파일 I/O에 `encoding="utf-8"` 명시 (5개 지점)
>
> **사용 방법:** Claude Code에서 이 fork를 플러그인 마켓플레이스로 등록하여 사용합니다.
> ```
> /plugin marketplace add SeoJaeWan/skills
> ```
>
> 상세 내용은 [skill-creator-windows-encoding-fix.md](./docs/skill-creator-windows-encoding-fix.md)를 참고하세요.

---

> **Note:** This repository contains Anthropic's implementation of skills for Claude. For information about the Agent Skills standard, see [agentskills.io](http://agentskills.io).

# Skills
Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. Skills teach Claude how to complete specific tasks in a repeatable way, whether that's creating documents with your company's brand guidelines, analyzing data using your organization's specific workflows, or automating personal tasks.

For more information, check out:
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Equipping agents for the real world with Agent Skills](https://anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

# About This Repository

This repository contains skills that demonstrate what's possible with Claude's skills system. These skills range from creative applications (art, music, design) to technical tasks (testing web apps, MCP server generation) to enterprise workflows (communications, branding, etc.).

Each skill is self-contained in its own folder with a `SKILL.md` file containing the instructions and metadata that Claude uses. Browse through these skills to get inspiration for your own skills or to understand different patterns and approaches.

Many skills in this repo are open source (Apache 2.0). We've also included the document creation & editing skills that power [Claude's document capabilities](https://www.anthropic.com/news/create-files) under the hood in the [`skills/docx`](./skills/docx), [`skills/pdf`](./skills/pdf), [`skills/pptx`](./skills/pptx), and [`skills/xlsx`](./skills/xlsx) subfolders. These are source-available, not open source, but we wanted to share these with developers as a reference for more complex skills that are actively used in a production AI application.

## Disclaimer

**These skills are provided for demonstration and educational purposes only.** While some of these capabilities may be available in Claude, the implementations and behaviors you receive from Claude may differ from what is shown in these skills. These skills are meant to illustrate patterns and possibilities. Always test skills thoroughly in your own environment before relying on them for critical tasks.

# Skill Sets
- [./skills](./skills): Skill examples for Creative & Design, Development & Technical, Enterprise & Communication, and Document Skills
- [./spec](./spec): The Agent Skills specification
- [./template](./template): Skill template

# Try in Claude Code, Claude.ai, and the API

## Claude Code
You can register this repository as a Claude Code Plugin marketplace by running the following command in Claude Code:
```
/plugin marketplace add anthropics/skills
```

Then, to install a specific set of skills:
1. Select `Browse and install plugins`
2. Select `anthropic-agent-skills`
3. Select `document-skills` or `example-skills`
4. Select `Install now`

Alternatively, directly install either Plugin via:
```
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

After installing the plugin, you can use the skill by just mentioning it. For instance, if you install the `document-skills` plugin from the marketplace, you can ask Claude Code to do something like: "Use the PDF skill to extract the form fields from `path/to/some-file.pdf`"

## Claude.ai

These example skills are all already available to paid plans in Claude.ai. 

To use any skill from this repository or upload custom skills, follow the instructions in [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude#h_a4222fa77b).

## Claude API

You can use Anthropic's pre-built skills, and upload custom skills, via the Claude API. See the [Skills API Quickstart](https://docs.claude.com/en/api/skills-guide#creating-a-skill) for more.

# Creating a Basic Skill

Skills are simple to create - just a folder with a `SKILL.md` file containing YAML frontmatter and instructions. You can use the **template-skill** in this repository as a starting point:

```markdown
---
name: my-skill-name
description: A clear description of what this skill does and when to use it
---

# My Skill Name

[Add your instructions here that Claude will follow when this skill is active]

## Examples
- Example usage 1
- Example usage 2

## Guidelines
- Guideline 1
- Guideline 2
```

The frontmatter requires only two fields:
- `name` - A unique identifier for your skill (lowercase, hyphens for spaces)
- `description` - A complete description of what the skill does and when to use it

The markdown content below contains the instructions, examples, and guidelines that Claude will follow. For more details, see [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills).

# Partner Skills

Skills are a great way to teach Claude how to get better at using specific pieces of software. As we see awesome example skills from partners, we may highlight some of them here:

- **Notion** - [Notion Skills for Claude](https://www.notion.so/notiondevs/Notion-Skills-for-Claude-28da4445d27180c7af1df7d8615723d0)
