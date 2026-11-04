# 설정과 실행 가이드

## 이번 작업의 범위

이번에는 저장소 복사·계정 설정 변경·문서 작성·로컬 검사만 수행했습니다. AWS CLI 조회, Terraform plan/apply/destroy, ECR 게시나 실제 배포는 실행하지 않았습니다.

## 설정된 본인 환경

| 항목 | 값 |
| --- | --- |
| GitHub | `yonginfra/Final_Project` |
| AWS 계정 | `143710973003` |
| 리전 / 프로필 | `ap-northeast-2` / `eks-user` |
| 웹·운영 도메인 | `server.syong.store`, `argocd.syong.store`, `grafana.syong.store` |
| Hosted Zone | `Z01939771EBX5SBI3T3FU` - 기존 로컬 코드에서 확인 |
| state 버킷 | `final-tfstate-143710973003-ap-northeast-2` |
| ECR | `143710973003.dkr.ecr.ap-northeast-2.amazonaws.com` |

위 값은 코드에 설정한 대상이며, 실제 AWS에서 존재·권한·인증서 유효 상태를 확인한 결과가 아닙니다.

## 배포 전 준비

1. `scripts/ops/environment.json`, 두 `backend.tf`, bootstrap과 ECR·IAM 참조를 함께 검토합니다. 기존 state를 사용한다면 상태 위치를 임의 변경하지 않습니다.
2. AWS 로그인·관리자 ARN·허용 공인 IP, 운영 알림 이메일·SES 발신/시험 수신 주소, 관리자 비밀번호와 앱 키를 준비합니다. 비밀 값은 커밋하지 않습니다.
3. GitHub 팀원 권한, 필요한 Secrets/Variables, OIDC 역할과 새 저장소 subject, 브랜치 보호를 준비합니다. 복사만으로 적용되지 않습니다.
4. 본인 ECR에 전체 이미지를 새로 빌드·게시하고 새 배포 PR을 승인합니다. 원본 digest·runId를 본인 계정의 유효한 배포 결과로 사용하지 않습니다.
5. 현재 전제에 맞는 도구 버전을 준비하고 상세 [PS1 가이드](../15-one-command-lab.md)를 읽습니다.

## AWS 없이 가능한 로컬 검사

```powershell
python scripts/ops/check_environment.py
python -m unittest discover -s tests -v
node --test app/msa/frontend/tests/*.test.cjs
terraform fmt -check -recursive terraform terraform-k8s terraform-state-bootstrap
```

아래 명령은 배포 준비가 끝난 뒤에만 사용합니다. 이 문서 작성 과정에서는 실행하지 않았습니다.

## 실제 설치

```powershell
.\Install-Lab.ps1 -Execute
```

state 준비 → AWS 기반 → EKS 내부 구성 → 최초 이미지·승인 → 앱·ALB·DNS 검증 순서입니다. 최초 설치 커밋 고정과 이후 main 전환은 [상세 가이드](../15-one-command-lab.md)를 따릅니다. plan을 확인한 범위에서만 진행합니다.

## 배포 후 검사

```powershell
.\Verify-Lab.ps1 -Mode Basic
```

Argo CD와 파드 상태뿐 아니라 실제 HTTPS·사용자 기능과 데이터도 확인합니다.

## 보존과 복원

```powershell
.\Preserve-Lab.ps1 -Execute
```

기존 데이터와 복구 정보를 기록하고 스냅샷 완료를 확인한 뒤 시험 대상 실행 환경을 제거합니다. 남긴 자원에는 비용이 발생합니다. 같은 사용자·PC에서 아래 명령으로 이어갑니다.

```powershell
.\Resume-Lab.ps1 -Execute
```

## 전체 정리

복원과 데이터가 더 이상 필요하지 않을 때만 별도로 실행합니다.

```powershell
.\Remove-Lab.ps1 -Execute -DiscardData
```

ALB·EBS를 Controller가 살아 있을 때 정리하고 이후 기반·스냅샷·잔여 자원·state 순으로 처리합니다. 기존 공용 Hosted Zone과 SES 인증 등은 삭제 대상과 구분합니다. 설치와 삭제 명령을 한 번에 붙여 넣지 않습니다.
