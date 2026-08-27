---
publish: true
tags: [지식, 데브옵스]
---

# Kubernetes를 활용한 배포

---

## 개요

**Kubernetes (K8s)**는 Google이 개발한 오픈소스 컨테이너 오케스트레이션 플랫폼으로, 대규모 컨테이너 애플리케이션의 배포, 스케일링, 관리를 자동화합니다.

### 특징

* **자동 스케일링**: 트래픽에 따라 자동으로 Pod 수 조절
* **자동 복구**: 실패한 컨테이너 자동 재시작 및 교체
* **로드 밸런싱**: 트래픽을 여러 Pod에 자동 분산
* **롤링 업데이트**: 무중단 배포 지원
* **서비스 디스커버리**: 동적 서비스 등록 및 발견
* **스토리지 관리**: 영구 저장소 자동 프로비저닝

### Kubernetes의 역사

* 2014년: Google이 Kubernetes 오픈소스 공개
* 2015년: Cloud Native Computing Foundation (CNCF)에 기부
* 현재: 컨테이너 오케스트레이션의 사실상 표준

---

## Kubernetes란?

> **Kubernetes (K8s)**는 컨테이너 오케스트레이션 플랫폼으로, 대규모 컨테이너 애플리케이션의 배포, 스케일링, 관리를 자동화합니다.

### 작동 원리

1. **선언적 구성**: 원하는 상태를 YAML로 정의
2. **Control Plane**: API Server가 상태 관리
3. **Controller**: 실제 상태를 원하는 상태로 조정
4. **Scheduler**: Pod를 적절한 노드에 배치
5. **kubelet**: 노드에서 Pod 관리 및 상태 보고

### Kubernetes 아키텍처 개요

**Control Plane (Master)**:
* 클러스터 전체 관리
* API Server, etcd, Scheduler, Controller Manager

**Worker Nodes**:
* 실제 워크로드 실행
* kubelet, kube-proxy, Container Runtime

---

## 핵심 요소와 개념

### 1. Pod (파드)

Pod는 Kubernetes에서 가장 작은 배포 단위입니다.

**핵심 개념**:
* 하나 이상의 컨테이너를 포함
* 컨테이너들은 같은 네트워크와 스토리지 공유
* 일시적(Ephemeral): 생성되고 삭제됨
* 고유한 IP 주소 할당

**Pod 생명주기**:
* Pending: 스케줄링 대기
* Running: 실행 중
* Succeeded: 정상 종료
* Failed: 실패
* Unknown: 상태 확인 불가

**심화 학습**:
* Init Containers: 메인 컨테이너 실행 전 초기화
* Sidecar 패턴: 로깅, 프록시 등 보조 컨테이너
* Pod Disruption Budget: 중단 허용 최소 Pod 수

### 2. Deployment (디플로이먼트)

Deployment는 Pod의 배포 및 관리를 담당합니다.

**핵심 기능**:
* ReplicaSet 관리: 지정된 수의 Pod 유지
* 롤링 업데이트: 무중단 업데이트
* 롤백: 이전 버전으로 복귀
* 스케일링: Pod 수 조절

**배포 전략**:
* RollingUpdate: 순차적 교체 (기본)
* Recreate: 전체 중지 후 재생성

**심화 학습**:
* Deployment Revision: 변경 이력 추적
* Rollout Status: 배포 상태 모니터링
* Deployment 전략 커스터마이징

### 3. Service (서비스)

Service는 Pod 집합에 대한 안정적인 네트워크 엔드포인트를 제공합니다.

**Service 유형**:
* **ClusterIP**: 클러스터 내부 접근 (기본)
* **NodePort**: 노드 IP:포트로 접근
* **LoadBalancer**: 외부 로드 밸런서 생성 (클라우드)
* **ExternalName**: 외부 서비스를 DNS로 참조

**작동 원리**:
* Label Selector로 Pod 선택
* 자동으로 Endpoints 생성
* kube-proxy가 트래픽 라우팅

**심화 학습**:
* Headless Service: DNS 기반 서비스 디스커버리
* Session Affinity: 같은 Pod로 라우팅
* Service Mesh (Istio): 고급 트래픽 관리

### 4. ReplicaSet (레플리카셋)

ReplicaSet은 지정된 수의 Pod 복제본을 유지합니다.

**핵심 기능**:
* Pod 수 유지: 지정된 replica 수 보장
* 자동 복구: 실패한 Pod 자동 교체

**Deployment와의 관계**:
* Deployment가 ReplicaSet을 관리
* 직접 사용하는 경우는 드묾

### 5. Namespace (네임스페이스)

Namespace는 클러스터를 논리적으로 분리합니다.

**기본 Namespace**:
* `default`: 기본 네임스페이스
* `kube-system`: 시스템 컴포넌트
* `kube-public`: 공개 정보
* `kube-node-lease`: 노드 하트비트

**사용 사례**:
* 환경 분리: dev, staging, prod
* 팀별 리소스 분리
* 리소스 할당량 관리

**심화 학습**:
* Resource Quota: 리소스 제한
* Network Policy: 네트워크 격리
* RBAC: 권한 관리

### 6. ConfigMap과 Secret

**ConfigMap**:
* 애플리케이션 설정 데이터
* YAML, JSON, 환경 변수 등
* Key-Value 쌍으로 관리

**Secret**:
* 민감한 정보 (비밀번호, 토큰 등)
* Base64 인코딩 (암호화 아님)
* 별도 관리 필요

**사용 방법**:
* 환경 변수로 주입
* 볼륨으로 마운트
* Init Container에서 사용

**심화 학습**:
* External Secrets Operator: 외부 시크릿 관리 도구
* Sealed Secrets: 암호화된 Secret
* Vault 통합: HashiCorp Vault 연동

### 7. PersistentVolume (PV)와 PersistentVolumeClaim (PVC)

**PersistentVolume (PV)**:
* 클러스터의 스토리지 리소스
* 관리자가 프로비저닝

**PersistentVolumeClaim (PVC)**:
* 사용자가 스토리지를 요청
* 동적으로 PV 할당 가능

**스토리지 클래스**:
* 동적 프로비저닝 설정
* 성능별로 다른 스토리지 타입
* SSD, HDD 등

**심화 학습**:
* StatefulSet: 상태를 가진 애플리케이션 배포
* Volume Snapshot: 볼륨 스냅샷
* 스토리지 마이그레이션

### 8. Ingress (인그레스)

Ingress는 클러스터 외부에서 내부 Service로의 HTTP/HTTPS 접근을 관리합니다.

**기능**:
* URL 기반 라우팅
* SSL/TLS 종료
* 로드 밸런싱
* 가상 호스팅

**Ingress Controller**:
* Nginx Ingress Controller (가장 인기)
* Traefik
* Istio Gateway

**심화 학습**:
* Ingress Rules: 경로 기반 라우팅
* SSL/TLS 인증서 관리 (Cert-Manager)
* Canary 배포 (Istio/Nginx)

### 9. Horizontal Pod Autoscaler (HPA)

HPA는 CPU/메모리 사용량에 따라 Pod 수를 자동 조절합니다.

**메트릭**:
* CPU 사용률
* 메모리 사용률
* 커스텀 메트릭 (Prometheus 등)

**작동 원리**:
* Metrics Server에서 메트릭 수집
* 목표 사용률과 비교
* ReplicaSet 스케일 조정

**심화 학습**:
* Vertical Pod Autoscaler (VPA)
* Cluster Autoscaler: 노드 자동 스케일링
* Custom Metrics: Prometheus 통합

### 10. DaemonSet과 StatefulSet

**DaemonSet**:
* 모든 노드에 Pod 실행
* 로깅 에이전트, 모니터링 에이전트 등

**StatefulSet**:
* 상태를 가진 애플리케이션 배포
* 순서 있는 배포 및 스케일링
* 안정적인 네트워크 ID
* 영구 스토리지

---

## Kubernetes의 주요 기능

* **자동 스케일링**: 트래픽에 따라 Pod 수 조절
* **자동 복구**: 실패한 컨테이너 자동 재시작
* **로드 밸런싱**: 트래픽을 여러 Pod에 분산
* **롤링 업데이트**: 무중단 배포
* **서비스 디스커버리**: 자동 서비스 등록 및 발견
* **스토리지 관리**: 영구 저장소 연결

---

## Kubernetes 아키텍처

### 클러스터 구성 요소

| 구성 요소 | 설명 |
| --------- | ---- |
| **Master Node** | 클러스터 제어 및 관리 |
| **Worker Node** | 실제 애플리케이션 실행 |
| **Pod** | 하나 이상의 컨테이너 그룹 |
| **Service** | Pod에 대한 안정적인 네트워크 엔드포인트 |
| **Deployment** | Pod의 배포 및 관리를 담당 |
| **Namespace** | 리소스의 논리적 분리 |

### Master Node 컴포넌트

* **API Server**: 클러스터의 통신 중심
* **etcd**: 클러스터 상태 저장소
* **Scheduler**: Pod를 적절한 노드에 배치
* **Controller Manager**: 클러스터 상태 관리

### Worker Node 컴포넌트

* **kubelet**: 노드에서 Pod 관리
* **kube-proxy**: 네트워크 프록시 및 로드 밸런싱
* **Container Runtime**: Docker, containerd 등

---

## 기본 리소스

### Pod

가장 작은 배포 단위:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

### Deployment

Pod의 배포 및 관리를 담당:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 3000
```

### Service

Pod에 대한 네트워크 접근 제공:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
```

---

## 배포 실전

### 1. 이미지 빌드 및 푸시

```bash
# 이미지 빌드
docker build -t my-app:v1.0.0 .

# 레지스트리에 푸시 (예: Docker Hub)
docker tag my-app:v1.0.0 username/my-app:v1.0.0
docker push username/my-app:v1.0.0
```

### 2. Deployment 생성

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: username/my-app:v1.0.0
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "production"
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

### 3. Service 생성

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 3000
    protocol: TCP
```

### 4. 배포 실행

```bash
# Deployment 생성
kubectl apply -f deployment.yaml

# Service 생성
kubectl apply -f service.yaml

# 상태 확인
kubectl get pods
kubectl get services
kubectl get deployments

# 로그 확인
kubectl logs -f deployment/my-app-deployment
```

---

## 롤링 업데이트

### 업데이트 방법

```bash
# 이미지 업데이트
kubectl set image deployment/my-app-deployment my-app=username/my-app:v2.0.0

# 또는 YAML 파일 수정 후
kubectl apply -f deployment.yaml

# 업데이트 상태 확인
kubectl rollout status deployment/my-app-deployment

# 이전 버전으로 롤백
kubectl rollout undo deployment/my-app-deployment

# 특정 리비전으로 롤백
kubectl rollout undo deployment/my-app-deployment --to-revision=2

# 업데이트 이력 확인
kubectl rollout history deployment/my-app-deployment
```

### Deployment 전략

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
```

---

## 스케일링

### 수동 스케일링

```bash
# Replica 수 조정
kubectl scale deployment/my-app-deployment --replicas=5
```

### 자동 스케일링 (HPA)

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

```bash
# HPA 생성
kubectl apply -f hpa.yaml

# HPA 상태 확인
kubectl get hpa
```

---

## ConfigMap과 Secret

### ConfigMap

애플리케이션 설정 관리:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
data:
  config.yaml: |
    database:
      host: localhost
      port: 5432
```

```yaml
# Deployment에서 사용
envFrom:
- configMapRef:
    name: my-app-config
```

### Secret

민감한 정보 관리:

```bash
# Secret 생성
kubectl create secret generic my-app-secret \
  --from-literal=username=admin \
  --from-literal=password=secret123
```

```yaml
# Deployment에서 사용
envFrom:
- secretRef:
    name: my-app-secret
```

---

## 영구 저장소 (Persistent Volume)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

```yaml
# Pod에서 사용
volumes:
- name: storage
  persistentVolumeClaim:
    claimName: my-pvc
```

---

## 주요 kubectl 명령어

### 기본 명령

```bash
kubectl get pods                    # Pod 목록
kubectl get services               # Service 목록
kubectl get deployments            # Deployment 목록
kubectl describe pod <pod-name>    # Pod 상세 정보
kubectl logs <pod-name>            # 로그 확인
kubectl exec -it <pod-name> -- /bin/bash  # Pod 접속
kubectl delete pod <pod-name>      # Pod 삭제
kubectl apply -f <file.yaml>       # 리소스 생성/업데이트
kubectl delete -f <file.yaml>      # 리소스 삭제
```

### 디버깅

```bash
kubectl get events                 # 이벤트 확인
kubectl top pods                   # 리소스 사용량
kubectl port-forward pod/<pod-name> 8080:80  # 포트 포워딩
```

---

## 네임스페이스

```bash
# 네임스페이스 생성
kubectl create namespace production

# 특정 네임스페이스에 배포
kubectl apply -f deployment.yaml -n production

# 네임스페이스별 리소스 확인
kubectl get pods -n production
```

---

## Kubernetes 배포 도구

### Minikube (로컬 테스트)

```bash
# Minikube 시작
minikube start

# 상태 확인
kubectl cluster-info
```

### kubeadm (온프레미스 클러스터)

* 프로덕션급 클러스터 구축
* Master/Worker 노드 구성

### 클라우드 서비스

* **Amazon EKS** (AWS)
* **Google GKE** (GCP)
* **Azure AKS** (Azure)
* **DigitalOcean Kubernetes**

---

---

## 심화 학습 포인트

### 1. Kubernetes 네트워킹

**Pod 네트워크**:
* 각 Pod는 고유 IP 주소
* 클러스터 내 Pod 간 직접 통신
* CNI (Container Network Interface) 플러그인 사용

**서비스 네트워킹**:
* ClusterIP: 가상 IP 할당
* kube-proxy: iptables 또는 IPVS 사용
* DNS 기반 서비스 디스커버리

**심화 학습**:
* CNI 플러그인 선택 (Calico, Flannel, Cilium 등)
* Network Policy: 네트워크 격리 및 보안
* Service Mesh: Istio, Linkerd

### 2. 보안 모범 사례

**RBAC (Role-Based Access Control)**:
* Role: 네임스페이스 레벨 권한
* ClusterRole: 클러스터 레벨 권한
* ServiceAccount: 애플리케이션 인증

**Pod Security Standards**:
* Privileged: 제한 없음
* Baseline: 최소 권한
* Restricted: 엄격한 제한

**심화 학습**:
* Admission Controllers
* Pod Security Policy (deprecated, Pod Security Standards로 대체)
* OPA Gatekeeper: 정책 강제

### 3. 리소스 관리

**Resource Requests와 Limits**:
```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"
```

**Resource Quota**:
* 네임스페이스별 리소스 제한
* CPU, 메모리, Pod 수 등

**심화 학습**:
* LimitRange: 기본 리소스 제한
* QoS 클래스: Guaranteed, Burstable, BestEffort
* 리소스 스케줄링 최적화

### 4. 모니터링 및 로깅

**메트릭 수집**:
* Metrics Server: 기본 메트릭
* Prometheus: 상세 메트릭 수집
* Grafana: 시각화

**로깅**:
* EFK Stack (Elasticsearch, Fluentd, Kibana)
* Loki: 경량 로그 집계

**심화 학습**:
* APM (Application Performance Monitoring)
* 분산 추적 (Jaeger, Zipkin)
* 알림 (AlertManager)

### 5. CI/CD 통합

**GitOps**:
* ArgoCD: GitOps 배포 도구
* Flux: CNCF GitOps 도구
* 코드로 인프라 및 애플리케이션 관리

**Helm**:
* Kubernetes 패키지 매니저
* Chart로 애플리케이션 패키징
* 템플릿과 값 분리

**심화 학습**:
* Helm Chart 작성
* Helm Repository 운영
* GitOps 워크플로우 구성

### 6. 고급 배포 전략

**Blue-Green 배포**:
* 두 개의 Deployment 준비
* Service Selector 변경으로 전환

**Canary 배포**:
* Istio를 이용한 트래픽 분할
* Flagger: 자동 Canary 배포

**A/B 테스트**:
* 서로 다른 버전 배포
* 사용자 그룹별 라우팅

**심화 학습**:
* Argo Rollouts: 고급 배포 전략
* Service Mesh를 이용한 트래픽 관리
* 자동 롤백 정책

### 7. 스케일링 전략

**Horizontal Pod Autoscaler (HPA)**:
* CPU/메모리 기반 스케일링
* 커스텀 메트릭 기반

**Vertical Pod Autoscaler (VPA)**:
* Pod의 리소스 요청 자동 조정
* 실제 사용량 기반

**Cluster Autoscaler**:
* 클러스터 노드 자동 추가/제거
* 클라우드 프로바이더 통합

**심화 학습**:
* Multi-metric HPA
* Predictive Scaling
* 비용 최적화

### 8. 트러블슈팅

**디버깅 도구**:
* `kubectl describe`: 리소스 상세 정보
* `kubectl logs`: Pod 로그 확인
* `kubectl exec`: 컨테이너 내부 접근
* `kubectl port-forward`: 포트 포워딩

**일반적인 문제**:
* ImagePullBackOff: 이미지 다운로드 실패
* CrashLoopBackOff: 컨테이너 반복 재시작
* Pending: 리소스 부족 또는 스케줄링 실패
* NotReady: 노드 상태 문제

**심화 학습**:
* Event 로그 확인
* 메트릭 분석
* 네트워크 연결 확인

### 9. 멀티 클러스터 관리

**Federation**:
* 여러 클러스터 통합 관리
* 리소스 동기화

**서비스 메시**:
* Istio Multi-cluster
* 서로 다른 클러스터 간 통신

**심화 학습**:
* 클러스터 간 트래픽 라우팅
* Disaster Recovery 전략
* 글로벌 배포

### 10. Kubernetes 운영

**클러스터 업그레이드**:
* 버전 관리 전략
* 롤링 업그레이드
* 데이터 마이그레이션

**백업 및 복구**:
* etcd 백업
* 리소스 백업 (Velero)
* Disaster Recovery 계획

**심화 학습**:
* 클러스터 모니터링
* 자동화 스크립트
* 문서화 및 Runbook

---

## 학습 체크리스트

### 초급
- [ ] Kubernetes 개념 이해 (Pod, Service, Deployment)
- [ ] kubectl 기본 명령어 사용
- [ ] 기본 리소스 생성 및 관리
- [ ] YAML 파일 작성
- [ ] 네임스페이스 사용

### 중급
- [ ] ConfigMap과 Secret 사용
- [ ] PersistentVolume 및 PVC 구성
- [ ] Ingress 설정
- [ ] Deployment 전략 (Rolling Update)
- [ ] HPA 설정 및 사용
- [ ] Resource Requests/Limits 설정

### 고급
- [ ] StatefulSet 및 DaemonSet 사용
- [ ] RBAC 구성
- [ ] Network Policy 구현
- [ ] Helm Chart 작성 및 사용
- [ ] 모니터링 및 로깅 구성 (Prometheus, Grafana)
- [ ] GitOps 워크플로우 구성 (ArgoCD)
- [ ] Service Mesh 구현 (Istio)
- [ ] 고급 배포 전략 (Canary, Blue-Green)
- [ ] 클러스터 운영 및 트러블슈팅
- [ ] 멀티 클러스터 관리

---

## 참고

* Kubernetes 공식 문서: https://kubernetes.io/docs/
* kubectl 치트시트: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* YAML 파일은 버전 관리에 포함시키는 것이 좋습니다
* 프로덕션 환경에서는 Resource limits 설정 필수
* 보안을 위해 RBAC 설정 권장
* Kubernetes 개념 참조: https://kubernetes.io/docs/concepts/
* Kubernetes Tasks: https://kubernetes.io/docs/tasks/

