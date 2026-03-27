# Code Review Harness

이 프로젝트는 Claude Code 기반 코드리뷰 자동화 파이프라인이다.

## 산출물 경로

모든 산출물은 `.review-artifacts/{branch-name}/` 하위에 저장한다.
branch명은 `git branch --show-current`로 resolve한다.

산출물 목록:
- `design-intent.md` — 설계의도 문서
- `code-quality-guide.md` — 평가기준 문서
- `pr-body.md` — PR 본문
- `review-comments.md` — 리뷰 코멘트

## 참조 문서

- `docs/code-convention.yaml` — 팀 코딩 컨벤션 (stacks 필드로 스택별 필터링 가능)
- `docs/adr.yaml` — Architecture Decision Records (stacks, date, status로 조회 가능)

## 파이프라인 규칙

1. Fork(worktree)로 실행하는 단계에서는 산출물 파일만 생성하고, 소스코드를 수정하지 않는다. (리뷰 반영 단계 제외)
2. 평가기준 생성 시 adr.md 전체를 포함하지 않는다. 이번 작업과 관련된 항목만 추출한다.
3. 코드리뷰는 반드시 평가기준에 근거해야 한다. 근거 없는 취향 리뷰를 남기지 않는다.
4. 사용자에게 확인을 요청할 때, 모호하여 구체화가 필요한 논의점을 반드시 함께 정리하여 전달한다.
5. base branch는 사용자가 명시하지 않으면 `main`을 기본으로 한다.
