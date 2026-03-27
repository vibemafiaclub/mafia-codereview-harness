# 평가기준 생성

작업 설계를 분석하고, `docs/adr.yaml`에서 관련 항목을 추출하여 `docs/code-convention.yaml`과 병합된 평가기준 문서를 생성한다.

## 실행 방식

이 skill은 worktree(fork)에서 실행된다.
ADR 분석은 sub-agent에게 위임한다.

## 절차

1. 현재 컨텍스트에서 작업의 설계 내용과 범위를 파악한다.
2. **Sub-agent를 호출**하여 다음을 수행한다:
   - `docs/adr.yaml` 전문을 읽는다.
   - 이번 작업의 설계 내용과 대조하여, 관련된 ADR 항목만 추출한다.
   - 관련 없는 항목은 제외하고, 관련 있는 항목은 이 작업에 어떻게 적용되는지 구체적으로 서술한다.
3. `docs/code-convention.yaml`을 읽고, 이번 작업의 stacks와 관련된 항목을 필터링한다.
4. 필터링된 code-convention(공통 기준) + 추출된 ADR 항목(작업별 기준)을 병합하여 `code-quality-guide.md` 초안을 작성한다.
5. 초안을 사용자에게 제시하되, **기준 적용 범위가 모호한 논의점**을 반드시 함께 정리한다.
   - 예: "ADR-003이 이 작업에 해당하는지?", "이 convention 항목의 적용 강도를 어떻게 할지?"
6. 사용자 피드백을 반영하여 확정한다.
7. 확정된 문서를 산출물 경로에 저장한다.

## Sub-agent 프롬프트

```
다음 작업 설계를 분석하고, adr.yaml에서 이 작업과 관련된 항목만 추출하라.
stacks 필드를 1차 필터로 활용하고, context/decision 내용으로 2차 판단하라.
관련 없는 항목은 제외하라.
관련된 항목은 이 작업에 구체적으로 어떻게 적용되는지 설명하라.

[작업 설계 내용]
[docs/adr.yaml 전문]
```

## 산출물 경로

```
.review-artifacts/{branch-name}/code-quality-guide.md
```

## 문서 구조

```markdown
# 평가기준

## 공통 기준 (Code Convention)
(docs/code-convention.yaml에서 가져온 항목)

## 이 작업에 적용되는 ADR
(sub-agent가 추출한 관련 ADR 항목 + 구체적 적용 방법)

### ADR-XXX: ...
- 적용 방법: ...
```
