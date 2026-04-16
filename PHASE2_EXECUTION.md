# Phase 2 실행 결과 (Repository & Knowledge Asset Design)

## 목표
현재 `SYSTEM_ARCHITECTURE_WORKFLOW.md`를 기준으로 Knowledge Bundle 최소 구성을 실제 파일로 구체화한다.

## 구현 항목
1. Knowledge asset frontmatter 템플릿 표준화
   - `templates/knowledge-asset-template.md`
2. knowledge-index 초안 설계
   - `templates/knowledge-index-template.yaml`
   - `metadata/knowledge-index.yaml`
3. Graphyfi output 연결 규칙 샘플
   - `samples/architecture/*.md`의 `related_graphyfi_paths`
   - `samples/graphyfi/graphs/*.json`

## 설계 결정
- 모든 샘플 문서는 `asset_id`, `related_docs`, `related_graphyfi_paths`를 포함한다.
- 인덱스는 `assets[]` 배열로 문서 타입/태그/연결 경로를 단일 조회 가능하도록 구성한다.
- Phase 3 파이프라인 조립을 위해 샘플 경로를 상대 경로로 고정한다.

## 다음 단계 (Phase 3 입력)
- `metadata/knowledge-index.yaml`을 기반으로 `collect_knowledge_assets` 단계 선택 규칙 정의
- `samples/graphyfi/graphs/impact/*.json`의 naming 규칙을 commit-hash 기반으로 강제
- `bundle_manifest.yaml` 스키마 초안 작성
