# 복사 출처와 환경 변경

## 복사 기준

- 원본: https://github.com/yobi84-sy/aws-msa-final-test2
- 브랜치: `main`
- 기준 커밋: `cc040cf5d8f621d19d3851aa1a59ae2229815e53`
- 대상: https://github.com/yonginfra/Final_Project
- 기준일: 2026-11-04

원본 main의 파일과 해당 main의 전체 Git 커밋 이력을 이어받습니다. 원본의 다른 작업 브랜치 전체를 새 브랜치로 게시하거나 PR·Actions 실행 기록을 복제한 것은 아닙니다. 원본 저장소는 수정하지 않습니다. 기존 MIT 라이선스와 팀 작업 출처를 유지합니다.

## 본인 환경으로 변경한 코드

| 항목 | 변경 |
| --- | --- |
| 계정·ARN·ECR·버킷 | `021158485482` → `143710973003` |
| 도메인 | `sy84.store` → `syong.store` |
| Hosted Zone | 기존 로컬 코드의 `Z01939771EBX5SBI3T3FU` 사용 |
| Argo CD·운영 스크립트 GitHub 참조 | `yonginfra/Final_Project` |
| 승인 검토 대상·CODEOWNERS | 새 저장소 소유자와 기존 팀원 목록 |
| OIDC subject | 새 저장소에서 조회한 아래 식별값 |

```text
repo:yonginfra@336437335/Final_Project@1445315716:ref:refs/heads/main
```

GitHub OIDC 설정의 `use_default=true`, `use_immutable_subject=true`, `sub_claim_prefix`를 읽어 확인했습니다. 이 조회는 GitHub API이며 AWS 인증이나 자원 조회가 아닙니다. 숫자 ID가 없는 문자열로 임의 단순화하지 않았습니다.

## 변경하지 않은 것과 별도 준비

- AWS 자원, DNS, SES 인증, 실제 state·비밀번호·토큰은 복사하거나 수정하지 않았습니다.
- 새 저장소의 Secrets·Variables·GitHub App·협업 권한·브랜치 보호는 자동 이전되지 않습니다.
- 기존 배포 digest와 runId는 원본 결과입니다. 이미지 바이트와 AWS 저장소가 자동 복제되는 것이 아닙니다.
- 현재 매니페스트는 본인 ECR 주소를 가리키지만, 이미지는 전체 빌드·게시와 새 배포 PR을 거쳐 준비해야 합니다.
- 과거 문서·증거의 계정과 도메인은 당시 기록이므로 일괄 바꾸지 않았습니다. 활성 코드·테스트의 환경값만 맞췄습니다.
- 원본 README와 배포 선언은 `docs/archive/`에 보존했습니다.

이 복사본으로 AWS 설치가 성공했다는 주장은 하지 않습니다. [로컬 검사](08-local-checks.md)와 이후 실제 배포 검증을 구분합니다.
