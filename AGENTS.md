# AGENTS.md

## 목적
이 저장소는 `SYSTEM_ARCHITECTURE_WORKFLOW.md`를 기반으로, AI KnowledgeOps + Graphyfi 중심의 아키텍처/운영 모델을 실행 가능한 형태로 정리하고 확장하기 위한 작업 공간이다.

## 작업 원칙
1. **Execution-first**: 모든 AI 작업은 실행 가능한 command/manifest 단위로 정의한다.
2. **Human-gated publishing**: 자동 생성 결과는 승인 전 source-of-truth에 직접 반영하지 않는다.
3. **Traceability-by-default**: 산출물마다 입력 문서, 커밋, 실행 명령 버전을 기록한다.
4. **Repo-separation**: `svc-* / know-* / gout-* / revw-*` 분리 원칙을 준수한다.
5. **MVP-pragmatism**: 초기에는 단순하고 재현 가능한 흐름(반자동 포함)을 우선한다.

## 문서 구성 가이드
- 핵심 설계/정책 문서는 Markdown으로 관리한다.
- 구조화된 메타데이터는 YAML/JSON으로 관리한다.
- 문서 간 참조는 `asset_id`, `related_docs`, `related_graphyfi_paths`를 우선 사용한다.

## Phase 기반 실행 Plan (영역별 주제)

### Phase 1 — Foundation & Governance (거버넌스/기준 수립)
- 목표: 저장소 운영 기준과 승인 정책을 확정한다.
- 범위:
  - 네이밍 규칙(`svc/know/gout/revw`) 확정
  - Human approval gate 정책 확정
  - 문서 타입 taxonomy(architecture/style/reference/build_runtime/troubleshooting/review_checklist) 확정
- 산출물:
  - 운영 규칙 문서
  - 리뷰/승인 책임 매트릭스

### Phase 2 — Repository & Knowledge Asset Design (저장소/지식자산 구조)
- 목표: Knowledge Bundle을 구성할 수 있는 최소 구조를 만든다.
- 범위:
  - Knowledge asset frontmatter 템플릿 표준화
  - `metadata/knowledge-index.yaml` 초안 설계
  - Graphyfi output 연결 규칙(`related_graphyfi_paths`) 정립
- 산출물:
  - 템플릿 문서 세트
  - 샘플 asset 및 index

### Phase 3 — Pipeline & Staging Assembly (파이프라인/스테이징 조립)
- 목표: 변경 감지부터 staging bundle 생성까지 자동화한다.
- 범위:
  - `detect_changes → identify_repo_scope → collect_knowledge/graphyfi → assemble_staging`
  - `bundle_manifest.yaml` 생성 로직 정의
  - 누락 입력에 대한 `blocked_manual_sync` 처리 정의
- 산출물:
  - CI stage 정의서
  - staging 디렉터리/매니페스트 규격

### Phase 4 — AI Command Execution & Review Artifacts (AI 실행/검토 산출물)
- 목표: opencode 실행과 리뷰 패킷 생성을 표준화한다.
- 범위:
  - `impact-analysis`, `architecture-update`, `review-artifact` 실행 플로우 확정
  - `proposed_changes.diff`, `rationale.md`, `review_checklist_filled.md` 포맷 확정
  - 실패 시 재시도/보류 전략 정의
- 산출물:
  - command 카탈로그
  - 리뷰 패킷 템플릿

### Phase 5 — Human Approval & Publishing Control (승인/배포 통제)
- 목표: 승인 후 반영 프로세스를 안전하게 운영한다.
- 범위:
  - 승인/반려 워크플로우 정의
  - publish stage 진입 조건과 충돌 처리(rebase/re-approve) 수립
  - 감사 대응을 위한 승인 이력 저장 정책 수립
- 산출물:
  - 승인 운영 절차서
  - 반영 체크리스트

### Phase 6 — Incident Analysis Integration (장애 분석 연계)
- 목표: 운영 이슈 분석에 knowledge + graphyfi를 연결한다.
- 범위:
  - incident manifest 입력 세트 정의
  - `opencode run incident-analysis` 결과 포맷 정의
  - fix/rollback 판단 근거 문서화 규칙 정의
- 산출물:
  - 장애 분석 템플릿
  - 의심 변경 포인트 추적 규격

### Phase 7 — Trace Metadata & Audit Readiness (추적성/감사 대응)
- 목표: 재현성과 감사 가능성을 운영 기본값으로 만든다.
- 범위:
  - trace yaml/json 필드 고정
  - 4종 commit hash(source/know/gout/review) 기록 강제
  - run 단위 trace_record_id 생성 규칙 확정
- 산출물:
  - trace schema
  - 감사 점검 체크리스트

### Phase 8 — MVP Validation & Scale-up (MVP 검증/확장)
- 목표: 단일 서비스 성공사례를 만든 뒤 다중 서비스로 확장한다.
- 범위:
  - `svc-payment-api` 수준의 파일럿 검증
  - KPI(업데이트 소요시간, 승인 리드타임, 누락률) 측정
  - `revw-*` 분리 저장소 도입 및 cross-repo 확장
- 산출물:
  - MVP 완료 보고서
  - 확장 로드맵

## Definition of Done (DoD)
- 변경 이벤트 기반으로 architecture update 초안이 자동 생성된다.
- 리뷰 아티팩트가 생성되고 사람 승인 후에만 반영된다.
- trace metadata에 입력 근거(문서/graphyfi/커밋/명령 버전)가 누락 없이 남는다.

## 운영 시 주의사항
- AI 결과를 정답으로 간주하지 않고, 근거 문서 링크/graph 근거를 항상 검토한다.
- Graphyfi output이 stale이면 업데이트를 보류하거나 최신화 후 재실행한다.
- 승인 없는 자동 반영은 금지한다.
