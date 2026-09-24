---
publish: true
tags: [지식, 클라우드]
---

# Openstack CLI 명령어

## 개요
Openstack CLI를 사용하여 이미지 생성, 플레이버 생성, 로그인 스크립트 등을 수행하는 방법을 정리합니다.

---

## 1. 로그인 스크립트
Openstack CLI를 사용하기 위해서는 인증 정보를 설정해야 합니다. 아래는 인증 스크립트 예제입니다:

```bash
export OS_AUTH_URL=http://<controller>:5000/v3
export OS_PROJECT_ID=<project_id>
export OS_PROJECT_NAME="<project_name>"
export OS_USER_DOMAIN_NAME="Default"
export OS_USERNAME="<username>"
export OS_PASSWORD="<password>"
export OS_REGION_NAME="RegionOne"
export OS_INTERFACE=public
export OS_IDENTITY_API_VERSION=3
```

위 스크립트를 실행하면 Openstack CLI 명령어를 사용할 수 있습니다.

---


```bash
# 프로젝트 목록 확인
openstack project list

# 이미지 목록 확인
openstack image list

# 인스턴스 목록 확인
openstack server list
```

**참고**: `source` 명령어는 현재 셸 세션에서만 환경 변수를 설정합니다. 새로운 터미널 세션에서는 다시 `source openstack_rc.sh`를 실행해야 합니다.


## 2. 이미지 생성
Openstack에서 이미지를 생성하려면 다음 명령어를 사용합니다:

### ISO 이미지 생성
```bash
openstack image create "<image_name>" \
  --file <iso_file_path> \
  --disk-format iso \
  --container-format bare \
  --public
```

### QCOW2 이미지 생성
```bash
openstack image create "<image_name>" \
  --file <qcow2_file_path> \
  --disk-format qcow2 \
  --container-format bare \
  --public
```

- `<image_name>`: 생성할 이미지의 이름
- `<iso_file_path>`: ISO 파일 경로
- `<qcow2_file_path>`: QCOW2 파일 경로

---

## 3. Flavor 생성
플레이버는 가상 머신의 하드웨어 설정을 정의합니다. EC2 인스턴스 유형에 맞춰 다음 예시를 제공합니다:

### t2.micro
```bash
openstack flavor create --id auto --ram 1024 --disk 8 --vcpus 1 t2.micro
```

### t2.medium
```bash
openstack flavor create --id auto --ram 4096 --disk 20 --vcpus 2 t2.medium
```

### m5.large
```bash
openstack flavor create --id auto --ram 8192 --disk 40 --vcpus 2 m5.large
```

- `--ram`: RAM 크기 (MB)
- `--disk`: 디스크 크기 (GB)
- `--vcpus`: 가상 CPU 개수
- `<flavor_name>`: 생성할 플레이버의 이름

---

## 4. 네트워크 생성
Openstack에서 네트워크를 생성하려면 다음 명령어를 사용합니다:

### 기본 네트워크 생성
```bash
openstack network create <network_name>
```

- `<network_name>`: 생성할 네트워크의 이름

### 외부 네트워크 생성 (Provider Network)
외부 네트워크는 물리 네트워크와 연결되어 외부 인터넷에 접근할 수 있는 네트워크입니다:

```bash
openstack network create \
  --external \
  --provider-physical-network <physical_network_name> \
  --provider-network-type <network_type> \
  <network_name>
```

**옵션 설명:**
- `--external`: 외부 네트워크로 설정
- `--provider-physical-network`: 물리 네트워크 이름 (예: `physnet1`, `datacentre`)
- `--provider-network-type`: 네트워크 타입
  - `flat`: 단일 물리 네트워크 사용
  - `vlan`: VLAN 태깅 사용
  - `vxlan`: VXLAN 오버레이 네트워크
  - `gre`: GRE 터널 사용

**예제:**
```bash
# Flat 타입 외부 네트워크 생성
openstack network create \
  --external \
  --provider-physical-network physnet1 \
  --provider-network-type flat \
  external-network

# VLAN 타입 외부 네트워크 생성
openstack network create \
  --external \
  --provider-physical-network physnet1 \
  --provider-network-type vlan \
  --provider-segment 100 \
  external-vlan-network
```

### 서브넷 생성
서브넷을 추가하려면 다음 명령어를 사용합니다:

**기본 서브넷:**
```bash
openstack subnet create \
  --network <network_name> \
  --subnet-range <subnet_cidr> \
  <subnet_name>
```

**외부 네트워크 서브넷 (게이트웨이 포함):**
```bash
openstack subnet create \
  --network <network_name> \
  --subnet-range <subnet_cidr> \
  --gateway <gateway_ip> \
  --allocation-pool start=<start_ip>,end=<end_ip> \
  --dns-nameserver <dns_server> \
  <subnet_name>
```

**옵션 설명:**
- `--subnet-range`: 서브넷 CIDR (예: `192.168.1.0/24`)
- `--gateway`: 게이트웨이 IP 주소 (예: `192.168.1.1`)
- `--allocation-pool`: IP 할당 풀 범위 (예: `start=192.168.1.10,end=192.168.1.200`)
- `--dns-nameserver`: DNS 서버 주소 (예: `8.8.8.8`)
- `--subnet-name`: 생성할 서브넷의 이름

**예제:**
```bash
# 외부 네트워크 서브넷 생성
openstack subnet create \
  --network external-network \
  --subnet-range 192.168.0.0/24 \
  --gateway 192.168.0.1 \
  --allocation-pool start=192.168.0.10,end=192.168.0.199 \
  --dns-nameserver 8.8.8.8 \
  external-subnet
```

---

## 5. 보안 그룹 설정
보안 그룹을 생성하고 규칙을 추가하려면 다음 명령어를 사용합니다:

```bash
openstack security group create <security_group_name>
openstack security group rule create --protocol <protocol> --dst-port <port_range> --ingress <security_group_name>
```

- `<protocol>`: 프로토콜 (예: tcp, udp)
- `<port_range>`: 포트 범위 (예: 22, 80:443)
- `<security_group_name>`: 보안 그룹 이름

---

## 6. 인스턴스 생성
Openstack에서 인스턴스를 생성하려면 다음 명령어를 사용합니다:

```bash
openstack server create \
  --flavor <flavor_name> \
  --image <image_name> \
  --network <network_name> \
  --security-group <security_group_name> \
  --key-name <key_name> \
  <server_name>
```

- `<flavor_name>`: 사용할 플레이버 이름
- `<image_name>`: 사용할 이미지 이름
- `<network_name>`: 연결할 네트워크 이름
- `<security_group_name>`: 사용할 보안 그룹 이름
- `<key_name>`: SSH 키 이름
- `<server_name>`: 생성할 서버의 이름

---

위 명령어를 참고하여 Openstack 환경을 구성하고 관리할 수 있습니다.