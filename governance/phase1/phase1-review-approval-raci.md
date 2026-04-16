# Phase 1 리뷰/승인 책임 매트릭스 (RACI)

- 기준 문서: `SYSTEM_ARCHITECTURE_WORKFLOW.md`, `governance/phase1/phase1-operating-rules.md`
- 상태: `approved-for-mvp`
- 최종 업데이트: 2026-04-15 (UTC)

## 역할 정의

- **Service Owner (SO)**: 서비스 기능/도메인 책임자
- **Architecture Owner (AO)**: 아키텍처 문서 품질/정합성 책임자
- **KnowledgeOps Operator (KO)**: 파이프라인/스테이징/매니페스트 운영 담당
- **Reviewer (RV)**: 변경 검토 담당 (개발/운영/품질)
- **Security & Compliance Approver (SC)**: 규제/보안 영향 승인자

## RACI 매트릭스

| 활동 | SO | AO | KO | RV | SC |
|---|---|---|---|---|---|
| repo scope 매핑 검증 (`identify_repo_scope`) | C | C | **A/R** | I | I |
| Graphyfi output 최신성 확인 | C | C | **A/R** | I | I |
| knowledge bundle 조립/검증 | C | C | **A/R** | I | I |
| `opencode run impact-analysis` 실행 | I | C | **A/R** | I | I |
| `opencode run architecture-update` 실행 | C | C | **A/R** | I | I |
| 리뷰 패킷 생성 (`proposed_changes.diff` 등) | I | C | **A/R** | C | I |
| 일반 변경 리뷰 | C | **A** | I | **R** | I |
| 보안/컴플라이언스 영향 리뷰 | I | C | I | C | **A/R** |
| 최종 승인 (`human_approval`) | C | **A/R** | I | C | 조건부 A/R |
| publish 수행 (`publish_to_repo`) | I | A | **R** | I | I |
| trace metadata 기록/보존 | I | C | **A/R** | I | I |

## 운영 규칙

1. `publish_to_repo`는 AO 승인 없이는 실행할 수 없다.
2. 보안/규제 영향 변경은 AO 승인 + SC 승인 모두 필요하다.
3. KO는 실행 책임(R)을 가지지만 승인 권한(A)은 갖지 않는다.
4. 승인 반려 시 KO가 재실행 큐를 관리하고, AO가 재검토 조건을 문서화한다.

## 승인 수준 정의

- **Level 1 (일반 변경)**: AO 승인 1인
- **Level 2 (중요 아키텍처 변경)**: AO + RV(시니어) 승인
- **Level 3 (보안/규제 영향)**: AO + SC 공동 승인

## 감사 추적 최소 항목

각 run마다 다음을 `trace metadata`로 남긴다.

- run 식별자 (`trace_record_id`)
- 승인 수준(Level 1/2/3)
- 승인자 계정/시간(UTC)
- 승인 대상 artifact 경로
- 반려 시 사유 코드 및 재실행 링크
