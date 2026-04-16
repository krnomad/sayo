# Phase 3 실행 문서 (문서기반)

## 목적
Phase 3의 목표인 `detect_changes → identify_repo_scope → collect_knowledge/graphyfi → assemble_staging` 자동화를 문서 기준으로 즉시 실행 가능한 형태로 정의한다.

## 입력 기준 문서
- `SYSTEM_ARCHITECTURE_WORKFLOW.md`의 3장(원칙), 4장(아키텍처), 5장(repo 전략), 6장(knowledge bundle), 7장(workflow)을 실행 기준으로 사용
- 저장소 운영 원칙은 `AGENTS.md`를 준수

## 실행 범위
- CI stage 정의서 작성
- staging 디렉터리/매니페스트 규격 확정
- 누락 입력 발생 시 `blocked_manual_sync` 처리 규칙 명시

## 실행 순서 (Execution-first)
1. `ci-stage-definition.md`에 단계별 입력/출력/실패조건을 명시한다.
2. `staging-spec.md`에 staging 파일 구조와 파일별 역할을 고정한다.
3. `templates/bundle_manifest.example.yaml`을 기준 매니페스트로 사용한다.
4. 입력 누락 또는 stale graph 발생 시 `templates/blocked_manual_sync.example.yaml`를 생성하여 publish를 차단한다.

## 완료 기준 (Phase 3 DoD)
- 변경 이벤트 1건에 대해 staging bundle 1세트가 재현 가능하게 생성된다.
- `bundle_manifest`만으로 실행 컨텍스트(입력 문서, graph, 커밋, 명령 버전)를 복원할 수 있다.
- 누락 입력이 있으면 자동 publish 대신 `blocked_manual_sync` 상태로 전환된다.
