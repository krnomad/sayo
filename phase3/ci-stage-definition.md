# Phase 3 CI Stage 정의서

## Stage 0. init_context
- 목적: 실행 컨텍스트 초기화
- 입력: webhook 이벤트(payload), 실행 시각, 파이프라인 런 ID
- 출력: `run_context.json`
- 실패 조건: 필수 이벤트 필드 누락(repo, branch, commit)

## Stage 1. detect_changes
- 목적: source 변경 파일과 변경 유형 식별
- 입력: `run_context.json`, source repo commit range
- 처리:
  - 변경 파일 목록 추출
  - 변경 유형 분류(added/modified/deleted/renamed)
- 출력: `changes/changed_files.json`, `changes/diff_summary.md`
- 실패 조건: commit range 계산 실패

## Stage 2. identify_repo_scope
- 목적: `svc-*` 기준으로 `know-*`, `gout-*`, `revw-*` 파생 매핑
- 입력: source repo name
- 처리:
  - 접두어 규칙 기반 suffix 계산
  - 대상 repo 존재 여부 확인
- 출력: `scope/repo_scope.yaml`
- 실패 조건: 필수 연관 repo 미존재 또는 접근 권한 없음

## Stage 3. collect_knowledge
- 목적: 변경 컴포넌트와 연관된 knowledge asset 수집
- 입력: `scope/repo_scope.yaml`, `changes/changed_files.json`, `metadata/knowledge-index.yaml`(know repo)
- 처리:
  - component/domain tag 매칭
  - `related_docs` 체인 확장
- 출력: `inputs/knowledge/` 하위 문서 사본, `inputs/knowledge_selection.yaml`
- 실패 조건: 매칭된 문서 0건(정책상 최소 1건 요구 시)

## Stage 4. collect_graphyfi
- 목적: graphyfi output 수집 및 freshness 검증
- 입력: `scope/repo_scope.yaml`, knowledge의 `related_graphyfi_paths`
- 처리:
  - 대상 graph 파일 수집
  - 생성 시각/커밋 기준 stale 여부 판단
- 출력: `inputs/graphyfi/` 하위 파일, `inputs/graphyfi_status.yaml`
- 실패 조건:
  - 필수 graph 누락
  - stale 기준 초과

## Stage 5. assemble_staging
- 목적: opencode 실행 가능한 bundle 조립
- 입력: Stage 1~4 출력물
- 처리:
  - 표준 디렉터리 구조 생성
  - `bundle_manifest.yaml` 생성
  - trace 필드 채움(run id, command version, source/know/gout commit)
- 출력: `staging/<run_id>/` 전체 번들
- 실패 조건: manifest 필수 필드 누락

## Stage 6. guard_blocked_manual_sync
- 목적: 누락 입력/품질 미충족 시 publish 차단
- 입력: stage 실패/경고 상태
- 처리:
  - `blocked_manual_sync.yaml` 생성
  - 차단 사유 및 수동 조치 항목 기록
- 출력: `staging/<run_id>/blocked_manual_sync.yaml`
- 실패 조건: 없음(차단 아티팩트 생성이 성공 기준)

## Stage 7. handoff_to_phase4
- 목적: Phase 4 명령 실행 단계로 인계
- 입력: `bundle_manifest.yaml`
- 처리:
  - `opencode run impact-analysis`
  - `opencode run architecture-update`
  - `opencode run review-artifact`
- 출력: review package(별도 phase)
- 실패 조건: command version 불일치 또는 실행 스펙 미존재

---

## 상태 전이 규칙
- `READY_FOR_PHASE4`: Stage 0~5 성공, 차단 없음
- `BLOCKED_MANUAL_SYNC`: Stage 3~5 중 필수 입력 누락/오염/stale 발생
- `FAILED_PIPELINE`: 시스템 장애(권한/checkout/manifest 생성 오류)

## 최소 자동화 우선순위 (MVP)
1. detect_changes
2. identify_repo_scope
3. assemble_staging
4. collect_knowledge / collect_graphyfi 정교화
