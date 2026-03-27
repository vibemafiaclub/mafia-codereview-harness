# PR 본문 생성

구현 완료된 코드의 git diff와 spec 변경사항을 기반으로 PR 본문을 생성한다.

## 실행 방식

이 skill은 worktree(fork)에서 실행된다.

## 절차

1. base branch(기본: `main`) 대비 현재 branch의 git diff를 수집한다.
   ```
   git diff main...HEAD
   ```
2. 변경된 파일 목록과 각 파일의 변경 내용을 분석한다.
3. 아래 구조에 맞춰 PR 본문 초안을 작성한다.
4. 초안을 사용자에게 제시하되, **PR 설명에서 명확히 해야 할 논의점**을 반드시 함께 정리한다.
   - 예: "breaking change 여부", "마이그레이션 필요 여부", "관련 이슈 번호"
5. 사용자 피드백을 반영하여 확정한다.
6. 확정된 문서를 산출물 경로에 저장한다.

## 산출물 경로

```
.review-artifacts/{branch-name}/pr-body.md
```

## 문서 구조

```markdown
# PR: {제목}

## Summary
(이 PR이 해결하는 문제와 접근 방식 요약, 1-3문장)

## Changes
(변경된 파일/모듈 단위로 무엇이 어떻게 바뀌었는지)

### {모듈/영역}
- ...

## Breaking Changes
(없으면 "없음")

## Test Plan
- [ ] ...

## Related
(관련 이슈, ADR, 설계 문서 등)
```
