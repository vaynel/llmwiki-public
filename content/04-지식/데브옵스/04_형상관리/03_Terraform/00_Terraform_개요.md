---
publish: true
tags: [지식, 데브옵스]
---

# Terraform 개요

---

## Terraform이란?

> **Terraform**은 HashiCorp에서 개발한 오픈소스 Infrastructure as Code (IaC) 도구로, 클라우드 인프라를 선언적으로 정의하고 관리할 수 있게 해주는 도구입니다. 멀티 클라우드를 지원하며, 인프라의 생성, 수정, 삭제를 자동화합니다.

---

## Terraform의 특징

### 1. 선언적 구성 (Declarative Configuration)
* 원하는 상태를 선언
* Terraform이 자동으로 계획 및 실행
* 명령형 스크립트 불필요

### 2. 멱등성 (Idempotency)
* 여러 번 실행해도 동일한 결과
* 현재 상태와 비교하여 필요한 변경만 수행
* 안전한 반복 실행

### 3. 멀티 클라우드 지원
* AWS, Azure, GCP 등 주요 클라우드 지원
* 온프레미스 인프라도 지원
* 하나의 도구로 통합 관리

### 4. 상태 관리 (State Management)
* 인프라 상태 추적
* 변경사항 추적
* 협업 지원

### 5. 계획 및 적용 (Plan & Apply)
* 실행 전 변경사항 미리보기
* 안전한 검토 후 적용
* 롤백 지원

### 6. 모듈화
* 재사용 가능한 모듈
* 코드 재사용성 향상
* 표준화된 인프라

---

## Terraform 아키텍처

### 1. Terraform Core
* 구성 파일 파싱
* 실행 계획 생성
* 리소스 그래프 생성
* 프로바이더 호출

### 2. Providers (프로바이더)
* 클라우드 플랫폼과의 인터페이스
* AWS, Azure, GCP 등
* 커뮤니티 프로바이더

### 3. State (상태)
* 현재 인프라 상태 저장
* 변경사항 추적
* 리소스 매핑

### 4. Configuration Files (구성 파일)
* `.tf` 파일
* HCL (HashiCorp Configuration Language)
* 선언적 구성

---

## Terraform 핵심 개념

### 1. Provider (프로바이더)
클라우드 플랫폼과의 연결을 담당

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-northeast-2"
}
```

### 2. Resource (리소스)
인프라 구성 요소를 정의

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

### 3. Data Source (데이터 소스)
기존 리소스 정보 조회

```hcl
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  
  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}
```

### 4. Variable (변수)
재사용 가능한 값 정의

```hcl
variable "instance_type" {
  description = "EC2 인스턴스 타입"
  type        = string
  default     = "t2.micro"
}

variable "environment" {
  description = "환경 이름"
  type        = string
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "환경은 dev, staging, prod 중 하나여야 합니다."
  }
}
```

### 5. Output (출력)
리소스 정보 출력

```hcl
output "instance_id" {
  description = "EC2 인스턴스 ID"
  value       = aws_instance.web.id
}

output "instance_public_ip" {
  description = "EC2 인스턴스 공용 IP"
  value       = aws_instance.web.public_ip
}
```

### 6. Module (모듈)
재사용 가능한 구성 단위

```hcl
module "vpc" {
  source = "./modules/vpc"
  
  vpc_cidr = "10.0.0.0/16"
  environment = var.environment
}
```

---

## Terraform 설치

### Linux/macOS
```bash
# Homebrew (macOS)
brew install terraform

# 직접 다운로드
wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip
unzip terraform_1.6.0_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

### Windows
```bash
# Chocolatey
choco install terraform

# 직접 다운로드 후 PATH에 추가
```

### 설치 확인
```bash
terraform version
```

---

## Terraform 기본 명령어

### 초기화
```bash
# Terraform 초기화
terraform init

# 특정 백엔드 사용
terraform init -backend-config="bucket=my-terraform-state"

# 모듈 업데이트
terraform init -upgrade
```

### 계획 및 적용
```bash
# 실행 계획 생성
terraform plan

# 계획을 파일로 저장
terraform plan -out=tfplan

# 계획 적용
terraform apply

# 저장된 계획 적용
terraform apply tfplan

# 자동 승인 (주의)
terraform apply -auto-approve
```

### 상태 관리
```bash
# 상태 확인
terraform show

# 상태 목록
terraform state list

# 특정 리소스 상태 확인
terraform state show aws_instance.web

# 상태 새로고침
terraform refresh

# 상태 가져오기
terraform import aws_instance.web i-1234567890abcdef0
```

### 리소스 관리
```bash
# 리소스 삭제
terraform destroy

# 특정 리소스만 삭제
terraform destroy -target=aws_instance.web

# 자동 승인
terraform destroy -auto-approve
```

### 기타 명령어
```bash
# 포맷팅
terraform fmt

# 검증
terraform validate

# 그래프 생성
terraform graph | dot -Tsvg > graph.svg

# 출력 확인
terraform output
terraform output instance_id
```

---

## Terraform 구성 파일 예시

### 기본 구성
```hcl
# main.tf
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "terraform.tfstate"
    region = "ap-northeast-2"
  }
}

provider "aws" {
  region = var.aws_region
}

resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "${var.environment}-vpc"
  }
}

resource "aws_subnet" "public" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.public_subnet_cidr
  availability_zone = var.availability_zone
  
  map_public_ip_on_launch = true
  
  tags = {
    Name = "${var.environment}-public-subnet"
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.latest_amazon_linux.id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.public.id
  
  vpc_security_group_ids = [aws_security_group.web.id]
  
  user_data = <<-EOF
    #!/bin/bash
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
  EOF
  
  tags = {
    Name = "${var.environment}-web-server"
  }
}
```

### 변수 파일
```hcl
# variables.tf
variable "aws_region" {
  description = "AWS 리전"
  type        = string
  default     = "ap-northeast-2"
}

variable "environment" {
  description = "환경 이름"
  type        = string
}

variable "vpc_cidr" {
  description = "VPC CIDR 블록"
  type        = string
  default     = "10.0.0.0/16"
}

variable "public_subnet_cidr" {
  description = "퍼블릭 서브넷 CIDR"
  type        = string
  default     = "10.0.1.0/24"
}

variable "instance_type" {
  description = "EC2 인스턴스 타입"
  type        = string
  default     = "t2.micro"
}

variable "availability_zone" {
  description = "가용 영역"
  type        = string
  default     = "ap-northeast-2a"
}
```

### 변수 값 파일
```hcl
# terraform.tfvars
environment        = "dev"
aws_region         = "ap-northeast-2"
vpc_cidr           = "10.0.0.0/16"
public_subnet_cidr = "10.0.1.0/24"
instance_type      = "t2.micro"
```

### 출력 파일
```hcl
# outputs.tf
output "vpc_id" {
  description = "VPC ID"
  value       = aws_vpc.main.id
}

output "subnet_id" {
  description = "퍼블릭 서브넷 ID"
  value       = aws_subnet.public.id
}

output "instance_id" {
  description = "EC2 인스턴스 ID"
  value       = aws_instance.web.id
}

output "instance_public_ip" {
  description = "EC2 인스턴스 공용 IP"
  value       = aws_instance.web.public_ip
}
```

### 데이터 소스
```hcl
# data.tf
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  
  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
  
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}
```

---

## Terraform 모듈

### 모듈 구조
```
modules/
  vpc/
    main.tf
    variables.tf
    outputs.tf
    README.md
```

### 모듈 사용
```hcl
module "vpc" {
  source = "./modules/vpc"
  
  vpc_cidr       = "10.0.0.0/16"
  environment    = var.environment
  public_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
}
```

### 모듈 생성
```hcl
# modules/vpc/main.tf
resource "aws_vpc" "this" {
  cidr_block = var.vpc_cidr
  
  tags = {
    Name = "${var.environment}-vpc"
  }
}

# modules/vpc/variables.tf
variable "vpc_cidr" {
  type = string
}

variable "environment" {
  type = string
}

# modules/vpc/outputs.tf
output "vpc_id" {
  value = aws_vpc.this.id
}
```

---

## Terraform 상태 관리

### 로컬 상태
* 기본적으로 `terraform.tfstate` 파일에 저장
* 단일 사용자 환경에 적합
* Git에 커밋하지 않도록 주의

### 원격 상태 (Remote State)
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "terraform.tfstate"
    region         = "ap-northeast-2"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
}
```

### 상태 잠금 (State Locking)
* 동시 실행 방지
* DynamoDB, Consul 등 사용
* 데이터 손실 방지

### 상태 백업
* 정기적인 백업
* 버전 관리
* 복구 계획 수립

---

## Terraform 워크스페이스

### 워크스페이스 생성 및 사용
```bash
# 워크스페이스 목록
terraform workspace list

# 새 워크스페이스 생성
terraform workspace new dev
terraform workspace new staging
terraform workspace new prod

# 워크스페이스 전환
terraform workspace select dev

# 현재 워크스페이스 확인
terraform workspace show
```

### 워크스페이스별 변수
```hcl
# terraform.tfvars.dev
environment = "dev"
instance_type = "t2.micro"

# terraform.tfvars.prod
environment = "prod"
instance_type = "t3.large"
```

---

## Terraform 모범 사례

### 1. 코드 구조
* 모듈화된 구조
* 환경별 분리
* 명확한 네이밍

### 2. 상태 관리
* 원격 상태 사용
* 상태 잠금 활성화
* 정기적인 백업

### 3. 보안
* 비밀 정보는 변수로 관리
* Terraform Cloud/Enterprise의 변수 사용
* 최소 권한 원칙

### 4. 버전 관리
* 코드를 Git으로 관리
* 의미 있는 커밋 메시지
* 태그 사용

### 5. 테스트
* `terraform plan`으로 검증
* 스테이징 환경에서 테스트
* 점진적 배포

### 6. 문서화
* README 작성
* 변수 및 출력 설명
* 아키텍처 다이어그램

### 7. 리소스 태깅
```hcl
tags = {
  Environment = var.environment
  Project     = "my-project"
  ManagedBy   = "Terraform"
  Owner       = "devops-team"
}
```

---

## Terraform Cloud / Enterprise

### Terraform Cloud
* 상태 관리
* 원격 실행
* 협업 기능
* 정책 관리

### Terraform Enterprise
* 자체 호스팅
* 엔터프라이즈 기능
* 고급 보안

---

## 주요 프로바이더

### 클라우드 프로바이더
* **AWS**: `hashicorp/aws`
* **Azure**: `hashicorp/azurerm`
* **GCP**: `hashicorp/google`
* **OCI**: `oracle/oci`

### 인프라 프로바이더
* **Docker**: `kreuzwerker/docker`
* **Kubernetes**: `hashicorp/kubernetes`
* **VMware**: `hashicorp/vsphere`

### 기타 프로바이더
* **GitHub**: `integrations/github`
* **Datadog**: `datadog/datadog`
* **PagerDuty**: `pagerduty/pagerduty`

---

## Terraform의 장점

* **멀티 클라우드**: 하나의 도구로 여러 클라우드 관리
* **선언적**: 원하는 상태만 선언
* **멱등성**: 안전한 반복 실행
* **상태 관리**: 인프라 상태 추적
* **모듈화**: 코드 재사용
* **계획 기능**: 변경사항 미리 확인

---

## Terraform의 단점

* **학습 곡선**: HCL 문법 학습 필요
* **상태 파일**: 상태 파일 관리 복잡
* **제한적 로직**: 복잡한 로직 표현 어려움
* **프로바이더 의존**: 프로바이더 업데이트 필요

---

## Terraform 사용 사례

### 1. 인프라 프로비저닝
* 클라우드 리소스 생성
* 네트워크 구성
* 보안 그룹 설정

### 2. 환경 관리
* 개발, 스테이징, 프로덕션 환경
* 환경별 구성 관리
* 일관된 인프라

### 3. 멀티 클라우드
* 여러 클라우드 통합 관리
* 클라우드 간 마이그레이션
* 하이브리드 클라우드

### 4. 규정 준수
* 인프라 표준화
* 정책 적용
* 감사 추적

---

## 참고

* Terraform은 Infrastructure as Code의 강력한 도구입니다
* 작은 인프라부터 시작하여 점진적으로 확장하세요
* 모듈을 활용하여 코드 재사용성을 높이세요
* 상태 관리를 신중하게 계획하세요
* 정기적으로 `terraform plan`으로 검증하세요
* 보안을 위해 비밀 정보 관리를 철저히 하세요
