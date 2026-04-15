# Phase 1 운영 규칙서 (Foundation & Governance)

- 기준 문서: `SYSTEM_ARCHITECTURE_WORKFLOW.md`
- 상태: `approved-for-mvp`
- 적용 범위: `svc-*`, `know-*`, `gout-*`, `revw-*` 전체
- 최종 업데이트: 2026-04-15 (UTC)

## 1) 목적

Phase 1의 목표는 저장소 운영 기준과 승인 정책을 확정해, 이후 Phase(2~8)가 동일 규칙을 기준으로 실행되도록 만드는 것이다.

## 2) Repo 네이밍 규칙 (확정)

### 2.1 표준 패턴

- `svc-{service-name}`: 소스 코드/설정/테스트
- `know-{service-name}`: architecture/guide/reference/build_runtime/troubleshooting/review_checklist
- `gout-{service-name}`: Graphyfi output 전용
- `revw-{service-name}`: AI 생성 리뷰 패킷/승인 이력

예시:

- `svc-payment-api`
- `know-payment-api`
- `gout-payment-api`
- `revw-payment-api`

### 2.2 매핑 규칙

1. 파이프라인 입력 repo는 반드시 `svc-` 접두어를 가진다.
2. suffix를 추출해 `know/gout/revw` repo를 계산한다.
3. 파생 repo가 없으면 `identify_repo_scope` 단계에서 즉시 실패 처리한다.

## 3) Human Approval Gate 정책 (확정)

### 3.1 원칙

- 승인 없는 자동 반영은 금지한다.
- AI 산출물은 `revw-*`에 저장 후, 사람 승인 이후에만 `know-*`(또는 승인된 타겟)로 publish 가능하다.

### 3.2 Gate 정책

- Gate 이름: `human_approval`
- 위치: `generate_review_artifacts` 이후, `publish_to_repo` 이전
- 입력: `proposed_changes.diff`, `rationale.md`, `review_checklist_filled.md`, trace 초안
- 승인권자: `Architecture Owner` + 필요 시 `Security/Compliance Approver`

### 3.3 필수 차단 조건

다음 중 하나라도 참이면 publish 차단:

1. 리뷰 체크리스트 미완료
2. 입력 근거(asset_id / related_graphyfi_paths / source commit)가 누락
3. 보안/컴플라이언스 관련 변경인데 추가 승인 미확보
4. Graphyfi output stale 플래그 해소 전

## 4) 문서 타입 taxonomy (확정)

Phase 1에서 표준 taxonomy를 아래로 고정한다.

| doc_type | 설명 | 기본 저장 위치 (`know-*`) |
|---|---|---|
| architecture | 구조/컴포넌트/인터페이스 설계 | `architecture/` |
| style_guide | 구현/문서/리뷰 스타일 규칙 | `guides/` |
| reference | 정책/스펙/용어 참조 | `reference/` |
| build_runtime | 빌드/배포/런타임 운영 문서 | `runtime/` |
| troubleshooting | 장애 진단/대응 문서 | `ops/` |
| review_checklist | 리뷰 기준/체크리스트 | `review/` |

> 주의: `graphyfi_output`은 taxonomy 참고 타입으로 사용 가능하지만 소스오브트루스 문서 타입 분류에는 포함하지 않는다. 산출물 저장소는 `gout-*`가 기본이다.

## 5) Execution-first 적용 규칙 (MVP)

1. 모든 AI 작업은 `opencode run <command>` 형태로 실행한다.
2. 실행 전 `bundle_manifest.yaml`을 생성/검증한다.
3. 최소 입력 검증 실패 시 `blocked_manual_sync` 상태로 전환한다.
4. 동일 manifest 기준 재시도는 최대 1회로 제한한다.

## 6) Traceability-by-default 최소 필드

Phase 1에서 아래 필드를 필수로 강제한다.

- `trace_record_id`
- `source_repo`, `knowledge_repo`, `graphyfi_repo`, `review_repo`
- `source_commit_hash`
- `knowledge_assets[].asset_id`
- `graphyfi_inputs[].path`
- `opencode_command`
- `generated_timestamp`
- `review_status`, `reviewed_by`, `review_timestamp`

## 7) Phase 1 DoD 체크

- [x] repo 네이밍 규칙 확정
- [x] human approval gate 정책 확정
- [x] 문서 타입 taxonomy 확정
- [x] 운영 규칙 문서화 완료
- [x] 리뷰/승인 책임 매트릭스 별도 문서화 완료
