# 코드리뷰 파이프라인

전체 코드리뷰 파이프라인을 오케스트레이션한다.

## 사전 조건

- `docs/code-convention.yaml`과 `docs/adr.yaml`이 팀 상황에 맞게 작성되어 있어야 한다.
- 구현이 완료된 feature branch에서 실행한다.

## 파이프라인

### Step 1: 상태 점검

1. `git branch --show-current`로 현재 branch를 확인한다.
2. `git diff main...HEAD --stat`으로 변경 파일 목록을 확인한다.
3. 사용자에게 다음을 확인한다:
   - branch명
   - base branch (기본: `main`)
   - 변경 파일 수
4. `.review-artifacts/{branch-name}/` 디렉토리를 생성한다.

### Step 2: 설계의도 작성

1. **worktree(fork)를 생성**하여 `/write-intent` skill을 실행한다.
2. 초안과 함께 **모호한 논의점**을 사용자에게 제시한다.
3. 사용자 피드백을 반영하여 `design-intent.md`를 확정한다.
4. 산출물을 `.review-artifacts/{branch-name}/`에 저장한다.
5. worktree를 정리한다.

### Step 3: 평가기준 수립

1. **worktree(fork)를 생성**하여 `/gen-criteria` skill을 실행한다.
2. sub-agent가 `docs/adr.yaml`을 분석하여 관련 항목을 추출한다.
3. 초안과 함께 **기준 적용 범위가 모호한 논의점**을 사용자에게 제시한다.
4. 사용자 피드백을 반영하여 `code-quality-guide.md`를 확정한다.
5. 산출물을 `.review-artifacts/{branch-name}/`에 저장한다.
6. worktree를 정리한다.

### Step 4: PR 본문 생성

1. **worktree(fork)를 생성**하여 `/create-pr-body` skill을 실행한다.
2. git diff 기반으로 PR 본문 초안을 생성한다.
3. 초안과 함께 **명확히 해야 할 논의점**을 사용자에게 제시한다.
4. 사용자 피드백을 반영하여 `pr-body.md`를 확정한다.
5. 산출물을 `.review-artifacts/{branch-name}/`에 저장한다.
6. worktree를 정리한다.

### Step 5: 코드리뷰 실행

1. **sub-agent를 호출**하여 `/review` skill을 실행한다.
2. sub-agent는 다음을 입력받아 자동으로 리뷰한다:
   - `.review-artifacts/{branch-name}/code-quality-guide.md`
   - `.review-artifacts/{branch-name}/design-intent.md`
   - `.review-artifacts/{branch-name}/pr-body.md`
   - `git diff main...HEAD`
3. `review-comments.md`를 `.review-artifacts/{branch-name}/`에 저장한다.
4. **사용자 확인 없이 자동으로 진행한다.**

### Step 6: 리뷰 반영

1. **worktree(fork)를 생성**하여 `/reflect-review` skill을 실행한다.
2. 각 리뷰 코멘트의 수용/거부 판단을 사용자에게 제시한다.
3. 사용자 확인 후 코드를 수정한다.
4. QA를 수행한다 (빌드, 테스트).
5. 버그 발견 시 수정 루프를 반복한다.
6. 핵심 변경사항을 메인 컨텍스트에 보고한다.

### 완료

파이프라인이 완료되면 산출물 경로의 전체 파일 목록과 최종 상태를 사용자에게 보고한다.
