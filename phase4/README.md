# Phase 4 실행 패키지 (문서 기반)

본 디렉터리는 `SYSTEM_ARCHITECTURE_WORKFLOW.md`의 **Phase 4 — AI Command Execution & Review Artifacts**를 현재 문서 기준으로 즉시 실행할 수 있도록 구체화한 산출물이다.

## 1) 포함 산출물

- `command-catalog.yaml`: `impact-analysis`, `architecture-update`, `review-artifact` 실행 명세
- `runbook.md`: 표준 실행 플로우 + 실패 시 재시도/보류 전략
- `templates/proposed_changes.diff`: 리뷰 패킷 diff 템플릿
- `templates/rationale.md`: 변경 근거 문서 템플릿
- `templates/review_checklist_filled.md`: 체크리스트 pre-fill 템플릿

## 2) 실행 원칙

- Execution-first: 모든 실행은 `bundle_manifest.yaml` 입력을 기준으로 수행한다.
- Human-gated publishing: 본 패키지는 `revw-*` 산출물 생성까지만 다루며, publish는 사람 승인 이후 단계에서만 수행한다.
- Traceability-by-default: 각 커맨드 실행 시 `trace_record_id`, 입력 해시, 커맨드 버전을 기록한다.

## 3) 최소 실행 절차

1. `bundle_manifest.yaml` 준비
2. `command-catalog.yaml`의 순서대로 실행
3. `runbook.md`의 검증 규칙으로 산출물 점검
4. 결과를 `revw-*` 저장소에 커밋

