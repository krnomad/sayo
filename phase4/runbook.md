# Phase 4 표준 실행 Runbook

## 1. 목적

- `impact-analysis` → `architecture-update` → `review-artifact` 실행을 표준화한다.
- 리뷰 패킷(`proposed_changes.diff`, `rationale.md`, `review_checklist_filled.md`)의 일관된 형식을 보장한다.

## 2. 실행 전 검증 (Preflight)

아래 항목 중 하나라도 누락되면 실행을 중단하고 `blocked_manual_sync` 처리한다.

- `staging/manifest/bundle_manifest.yaml`
- `staging/input/changed_files.txt`
- `staging/input/graphyfi/impact-*.json`
- `staging/input/knowledge/*.md`

## 3. 실행 단계

### Step A. 영향도 분석

```bash
opencode run impact-analysis --manifest staging/bundle_manifest.yaml
```

검증 포인트:
- `staging/output/impact_report.md` 생성 여부
- `staging/output/impact.json` 생성 여부

### Step B. 아키텍처 업데이트 초안 생성

```bash
opencode run architecture-update --manifest staging/bundle_manifest.yaml
```

검증 포인트:
- `staging/output/architecture_patch.md` 생성 여부
- `staging/output/rationale.md` 생성 여부

### Step C. 리뷰 아티팩트 생성

```bash
opencode run review-artifact --input staging/output/
```

검증 포인트:
- `revw/runs/{run_id}/proposed_changes.diff`
- `revw/runs/{run_id}/rationale.md`
- `revw/runs/{run_id}/review_checklist_filled.md`

## 4. 실패 처리

1. **1차 실패**: 동일 manifest로 1회 재시도
2. **2차 실패**: `review_status=blocked_manual_sync` 기록
3. **보류 조치**:
   - 누락 입력 보강 후 재실행
   - Graphyfi stale이면 `gout-*` 최신화 후 재실행

## 5. 산출물 품질 기준

- `proposed_changes.diff`: 파일 단위 변경과 이유가 연결되어야 한다.
- `rationale.md`: 변경 근거 문서/graphyfi 경로/영향 범위가 모두 명시되어야 한다.
- `review_checklist_filled.md`: 승인/반려 판단 가능한 체크 항목이 pre-fill 되어야 한다.

