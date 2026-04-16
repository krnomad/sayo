# Phase 3 Staging 디렉터리/매니페스트 규격

## 1) 디렉터리 표준

```text
staging/
  <run_id>/
    bundle_manifest.yaml
    run_context.json
    changes/
      changed_files.json
      diff_summary.md
    scope/
      repo_scope.yaml
    inputs/
      knowledge/
      graphyfi/
      knowledge_selection.yaml
      graphyfi_status.yaml
    trace/
      trace_record.yaml
    blocked_manual_sync.yaml   # 필요 시에만 생성
```

## 2) `bundle_manifest.yaml` 필수 필드
- `bundle_id`: `bundle-<svc_repo>-<run_id>`
- `workflow_phase`: `phase3`
- `source_repo`, `know_repo`, `gout_repo`, `revw_repo`
- `source_commit`, `know_commit`, `gout_commit`
- `inputs.knowledge_assets[]`
- `inputs.graphyfi_outputs[]`
- `commands[]` (phase4 인계용)
- `trace.run_id`, `trace.command_catalog_version`, `trace.generated_at`
- `status` (`ready_for_phase4` | `blocked_manual_sync`)

## 3) `blocked_manual_sync.yaml` 생성 조건
다음 중 하나라도 참이면 생성한다.
1. knowledge asset 매칭 결과가 비어 있음
2. required graphyfi output 누락
3. graphyfi freshness 정책 위반(stale)
4. 필수 커밋 해시 획득 실패

## 4) `blocked_manual_sync.yaml` 필수 필드
- `block_id`
- `run_id`
- `detected_at`
- `reasons[]` (code/message/severity)
- `required_manual_actions[]`
- `owner`
- `next_retry_hint`

## 5) 검증 체크리스트
- 번들 내 모든 경로가 상대경로인지 확인
- manifest 목록과 실제 파일셋이 일치하는지 확인
- trace 필드의 커밋 해시가 실제 checkout과 일치하는지 확인
- `status=blocked_manual_sync`이면 publish stage 진입 금지
