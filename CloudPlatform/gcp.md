# GCP
# IAM
IAM을 사용하면 GCP 리소스에대한 세부적인 액세스 권한을 부여하고 특정 리소스에대한 액세를 방지.

최소 권한의 보안원칙을적용하여 필요한 권한만 부여 가능.

사용자 지정 역할을 생성하려면 iam.roles.create권한이 있어야 함.
- 조직 역할 관리자 :rolesiam.organizationRoleAdmin
- IAM 역할 관리자 :  roles/iam/roleAdmin
- IAM 보안 검토자 : roles/iam/SecurityReviewer

## 프로젝트 리소스에 대한 기본 역할
1. **roles/viewer**
```
상태에 영향을 주지 않는 읽기 전용 작업에 대한 권한이 있습니다. 예를 들면 기존 리소스 또는 데이터의 조회(수정 제외)가 여기에 해당합니다.
```
**2. roles/editor**
```
모든 뷰어 권한에 더해 기존 리소스 변경과 같이 상태를 변경하는 작업에 대한 권한까지 포함됩니다.
```
**3. roles/owner**
```
모든 편집자 권한 및 다음 작업에 대한 권한:

프로젝트 및 프로젝트 내의 모든 리소스에 대한 역할 및 관리

프로젝트에 대한 결제 설정
```
**roles/탐색**
```
폴더, 조직, Cloud IAM 정책을 포함한 프로젝트 계층 구조를 탐색할 수 있는 읽기 액세스 권한입니다. 프로젝트의 리소스를 볼 수 있는 권한은 포함 안함.
```

**CLI로 확인 (gui에서의 리스트 목록과 다르게나와 확인이 필요)**

- gcloud auth list : 내 계정 확인
- gcloud config list project : 리스트 확인
- gcloud config set project 짝꿍프로젝트id : 접근하려는 프로젝트 등록

**버킷정보 보기 (Storage)**

- gsutil ls gs://[버킷이름] →"저장소객체뷰어" 역할을 할당받은 버킷이름을 적어주면됨 권한이없으면 접근이 안됨
- gsutil cp gs://[버킷이름]/[파일명]  [복사할 경로]
- gsutil cp gs://nobreakclass03-bucket/cat1.jfif . → 해당 버킷파일을 내 현재 디렉토리에 옮기기
- ls로 확인

## 상황별 권한 주는 방법

1. 프로젝트 내의 모든 리소스 접근 : IAM → project viewer 역할 부여
2. 모든 스토리지 버킷의 접근 허용 : IAM → storage objet viewer(저장소 객체 뷰어) 역할 부여
3. 특정 버킷에 대한 접근 허용 : storage → 특정 버킷→ 권한 → storage object viewer 역할 부여


## 방화벽 설정 -네트워크 태그-

**과정**

vpc네트워크 → 기본 default network ,28개의 region 존재 → 방화벽 → 방화벽 규칙 생성(name : default-allow-https)

**네트워크 태그 사용**


### Role (compute engine 서비스)

- **네트워크 관리자** : 방화벽 규칙, SSL 인증서를 제외한 네트워킹 리소스를 생성, 수정, 삭제 할 수 있는 권한. ( 방화벽 규칙, SSL 인증서, 인스턴스에 대한 읽기 전용 액세스)
- **보안 관리자** : 방화벽 규칙, SSL 인증서, 인스턴스에 대해 생성, 수정, 삭제를 할 수 있는 권한

**목표**

네트워크관리자와 보안관리자로 방화벽 규칙을 확인, 삭제, 생성



 방화벽 default-web-allow 정책 삭제

``gcloud compute firewall-rules delete  [삭제할 rules]``

``gcloud compute firewall-rules delete  default-web-allow``


네트워크 관리자는 읽기만 가능하고 삭제,수정 권한이 없다.

->삭제할 수 없음

**삭제**를하려면 permission을 부여하는 것이 아니라 permission을 갖고있는 **역할을 부여**해야함

**보안 관리자 역할을** 부여

### 서비스 계정 생성 (보안관리자)

**과정**

role : compute engine 중 compute security manager  → key생성 → instance 2에 키 인증 통해 방화벽 정책 삭제 가능한지 확인

**세부 과정**

IAM 서비스 계정 → 서비스 계정 추가 → role: [compute engine/compute security manager] → 해당 서비스 선택후 key추가 → jason파일 다운 받음 → instance 2에서 작업
