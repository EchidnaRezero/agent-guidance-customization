---
name: writing-plans
description: 여러 단계 작업의 spec 또는 requirements가 있고 code를 만지기 전에 사용합니다.
---

# Writing Plans

## 개요

`brainstorming` overview design doc에서 시작해 구현 가능한 계획으로 바꿉니다. 실행할 engineer가 codebase context가 없고 판단력이 들쭉날쭉하다고 가정하고, 필요한 모든 정보를 문서화하세요. 어떤 file을 각 task에서 만질지, code, testing, 확인할 docs, test 방법을 포함합니다. 계획은 작은 task로 나눕니다. DRY. YAGNI. TDD. Frequent commits.

engineer는 숙련된 개발자지만 toolset이나 problem domain은 거의 모른다고 가정하세요. 좋은 test design도 잘 모른다고 가정하세요.

**시작할 때 알림:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** 이 작업은 `brainstorming` skill이 만든 전용 worktree에서 실행해야 합니다.

**계획 저장 위치:** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`

- 사용자 선호 plan 위치가 있으면 이 기본값보다 우선합니다.

## 범위 확인

`brainstorming` overview design doc이 purpose, user, scope, output shape, benefit, completion criteria의 source of truth입니다.

spec이 여러 독립 subsystem을 포함하면 `brainstorming` 중 sub-project spec으로 나뉘었어야 합니다. 그렇지 않았다면 subsystem마다 하나의 plan으로 나누자고 제안하세요. 각 plan은 자체적으로 작동하고 test 가능한 software를 만들어야 합니다.

planning 현실이 `brainstorming`에서 합의한 boundary 또는 output shape를 깨면, 억지로 plan을 밀어붙이지 말고 `brainstorming`으로 돌아가세요.

## 필수 출력

`writing-plans`는 최소 두 artifact를 만들어야 합니다.

1. technical design document
2. implementation task board 또는 plan

technical design document는 다음을 다뤄야 합니다.

- Project structure
- File structure
- 관련이 있을 때 class structure
- 주요 function과 behavior

implementation task board는 다음을 다뤄야 합니다.

- Implementation order
- Task units
- Verification order

## 계획 결정

구현 시작 전에 다음 결정을 plan에 고정하세요.

- Tech stack
- Security level
- Debug logging strategy
- Test strategy
- Local environment approach
- Git usage plan

이 결정들은 `brainstorming` overview design doc과 맞아야 합니다. 결정이 purpose, user, scope, output shape, benefit, completion criteria를 바꾸면 작업을 `brainstorming`으로 되돌리세요.

## 파일 구조

task를 정의하기 전에 어떤 file을 만들거나 수정할지, 각 file이 무엇을 책임지는지 map out하세요. 여기에서 decomposition 결정이 고정됩니다.

- 명확한 boundary와 잘 정의된 interface를 가진 design unit을 만드세요. 각 file은 하나의 명확한 책임을 가져야 합니다.
- context 안에 담을 수 있는 code를 더 잘 추론할 수 있고, 집중된 file일수록 edit가 더 reliable합니다. 너무 많은 일을 하는 큰 file보다 작고 집중된 file을 선호하세요.
- 함께 바뀌는 file은 함께 두세요. technical layer가 아니라 responsibility 기준으로 나누세요.
- 기존 codebase에서는 established pattern을 따르세요. codebase가 큰 file을 사용한다고 해서 일방적으로 구조를 바꾸지 마세요. 다만 수정할 file이 너무 커졌다면 plan에 split을 포함하는 것은 합리적입니다.

이 구조가 task decomposition을 결정합니다. 각 task는 독립적으로 의미 있는 self-contained changes를 만들어야 합니다.

## 작은 Task 단위

**각 step은 하나의 action입니다(2-5분):**

- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**모든 plan은 반드시 이 header로 시작해야 합니다.**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```powershell
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Placeholder 금지

모든 step에는 engineer에게 필요한 실제 content가 있어야 합니다. 다음은 **plan failures**입니다. 절대 쓰지 마세요.

- "TBD", "TODO", "implement later", "fill in details"
- "Add appropriate error handling" / "add validation" / "handle edge cases"
- "Write tests for the above" (실제 test code 없음)
- "Similar to Task N" (code를 반복하세요. engineer는 task를 순서와 다르게 읽을 수 있습니다.)
- 방법을 보여주지 않고 할 일만 설명하는 step. code step에는 code block이 필요합니다.
- 어떤 task에서도 정의되지 않은 type, function, method 참조

## 기억할 것

- 항상 정확한 file path
- 모든 step에 complete code. step이 code를 바꾸면 code를 보여주세요.
- expected output이 있는 정확한 command
- DRY, YAGNI, TDD, frequent commits

## Self-Review

완전한 plan을 작성한 뒤, spec을 새 눈으로 보고 plan과 대조하세요. 이것은 직접 실행하는 checklist이며 subagent dispatch가 아닙니다.

**1. Spec coverage:** `brainstorming` overview design doc과 각 requirement를 훑으세요. 구현하는 task를 각각 가리킬 수 있나요? gap을 나열하세요.

**2. Drift check:** plan이 purpose, user, scope, output shape, benefit, completion criteria 면에서 `brainstorming` overview doc과 여전히 맞는지 확인하세요.

**3. Placeholder scan:** "Placeholder 금지" section의 red flag pattern이 plan에 있는지 검색하세요. 발견하면 고치세요.

**4. Type consistency:** 뒤 task에서 사용한 type, method signature, property name이 앞 task에서 정의한 것과 일치하나요? Task 3에서는 `clearLayers()`인데 Task 7에서는 `clearFullLayers()`라면 bug입니다.

issue를 발견하면 inline으로 고치세요. 다시 review할 필요 없이 고치고 진행하세요. task가 없는 spec requirement를 발견하면 task를 추가하세요.

## 실행 Handoff

plan을 저장한 뒤 실행 방식을 제안하세요.

**"Plan complete and saved to `docs/superpowers/plans/<filename>.md`. Two execution options:**

**1. Subagent-Driven (recommended)** - I dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?"**

**Subagent-Driven이 선택되면:**

- **REQUIRED SUB-SKILL:** `superpowers:subagent-driven-development`를 사용하세요.
- task마다 fresh subagent + two-stage review

**Inline Execution이 선택되면:**

- **REQUIRED SUB-SKILL:** `superpowers:executing-plans`를 사용하세요.
- review checkpoint가 있는 batch execution
