# 🚀 AWS DevOps Pipeline Project

## 📋 Proje Özeti
Bu proje, modern DevOps pratiklerini kullanarak Spring Boot uygulamasının tam otomatik CI/CD pipeline'ını içerir. Jenkins, Docker, Kubernetes, SonarQube, Trivy ve AWS EKS teknolojileri kullanılarak geliştirilmiştir.

## 🛠️ Teknoloji Stack'i

| Teknoloji | Versiyon | Açıklama |
|-----------|----------|----------|
| **Java** | 21 | Backend programlama dili |
| **Spring Boot** | 3.5.5 | Web framework |
| **Maven** | 3.9+ | Build tool |
| **Docker** | Latest | Containerization |
| **Kubernetes** | 1.30+ | Container orchestration |
| **Jenkins** | 2.400+ | CI/CD automation |
| **SonarQube** | 9.0+ | Code quality analysis |
| **Trivy** | Latest | Security scanning |
| **AWS EKS** | 1.30+ | Managed Kubernetes service |

## 🏗️ Sistem Mimarisi

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Developer     │    │     GitHub      │    │    Jenkins      │
│                 │    │   Repository    │    │     Server      │
│  Code Push      │───▶│                 │───▶│                 │
│                 │    │  Webhook Trigger│    │  Pipeline Start │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                       │
                                                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Docker Hub    │    │   SonarQube     │    │     Trivy       │
│                 │    │                 │    │                 │
│  Image Storage  │◀───│  Code Quality   │◀───│ Security Scan   │
│                 │    │   Analysis      │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │
         ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   AWS EKS       │    │   Kubernetes    │    │   Application   │
│                 │    │                 │    │                 │
│  Cluster        │───▶│   Deployment    │───▶│   Production    │
│  Management     │    │   & Service     │    │   Environment   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🔄 CI/CD Pipeline Akışı

### 1. **Source Control Management (SCM)**
- GitHub repository'den kod çekme
- Main branch'den otomatik trigger

### 2. **Test Stage**
```bash
mvn test
```
- Unit testlerin çalıştırılması
- Test coverage analizi

### 3. **Build Stage**
```bash
mvn clean install
```
- Maven ile proje build
- JAR dosyası oluşturma

### 4. **Code Quality Analysis**
```bash
mvn sonar:sonar
```
- SonarQube ile kod kalitesi analizi
- Code smell, bug, vulnerability tespiti
- Quality Gate kontrolü

### 5. **Docker Build & Push**
```bash
docker build -t onurguler18/devops-03-pipeline-aws:1.0.${BUILD_NUMBER}
docker push onurguler18/devops-03-pipeline-aws:1.0.${BUILD_NUMBER}
docker push onurguler18/devops-03-pipeline-aws:latest
```
- Docker image oluşturma
- DockerHub'a push

### 6. **Security Scanning**
```bash
trivy image onurguler18/devops-03-pipeline-aws:latest
```
- Trivy ile güvenlik taraması
- HIGH ve CRITICAL seviye vulnerability kontrolü

### 7. **Kubernetes Deployment**
```bash
kubectl apply -f deployment-service.yaml
```
- EKS cluster'a deployment
- Service oluşturma

### 8. **Cleanup**
- Eski Docker image'ları temizleme
- Disk alanı optimizasyonu

## 📁 Proje Yapısı

```
aws-pipeline/
├── src/
│   ├── main/
│   │   ├── java/com/onurguler/
│   │   │   ├── AppMain.java              # Spring Boot main class
│   │   │   └── controller/
│   │   │       └── DevOpsController.java # REST API endpoints
│   │   └── resources/
│   │       └── application.properties    # App configuration
│   └── test/                             # Test classes
├── target/                               # Build artifacts
├── Dockerfile                           # Docker configuration
├── deployment.yaml                      # K8s deployment
├── service.yaml                         # K8s service
├── Jenkinsfile                          # CI/CD pipeline
├── pom.xml                             # Maven configuration
└── README.md                           # Project documentation
```

## 🏗️ Infrastructure Setup

### AWS Instance Types (t4g.xlarge referans alınarak)

| Makine | Instance Type | vCPU | RAM | Storage | Açıklama |
|--------|---------------|------|-----|---------|----------|
| Jenkins Master | t4g.xlarge | 4 | 16GB | 15GB | Ana CI/CD server |
| Jenkins Agent | t4g.large | 2 | 8GB | 15GB | Build işlemleri için |
| SonarQube | t4g.medium | 2 | 4GB | 15GB | Code quality analysis |
| EKS Bootstrap | t4g.small | 2 | 2GB | 15GB | Cluster management |
| EKS Nodes | t4g.medium | 2 | 4GB | 15GB | Application workloads |

### Makine 1: Jenkins Master Server

#### AWS EC2 Instance (t4g.xlarge)
1. **Instance Type**: `t4g.xlarge` (4 vCPU, 16GB RAM, ARM64)
2. **AMI**: Ubuntu Server 22.04 LTS (ARM64)
3. **Storage**: 15GB GP3 EBS
4. **Network**: VPC with internet gateway

### 🛠️ Kurulum Adımları

#### ☕ Java 21 & Maven
- Sistem güncellemesi ve Java 21 JDK kurulumu
- Maven build tool kurulumu
- Sürüm kontrolü ve doğrulama

#### 🚀 Jenkins Setup
- Jenkins repository konfigürasyonu
- Jenkins servisi kurulumu ve başlatma
- Admin panel erişimi ve ilk konfigürasyon

### Makine 2: Jenkins Agent Server

#### AWS EC2 Instance (t4g.large)
1. **Instance Type**: `t4g.large` (2 vCPU, 8GB RAM, ARM64)
2. **AMI**: Ubuntu Server 22.04 LTS (ARM64)
3. **Storage**: 15GB GP3 EBS
4. **Network**: VPC with internet gateway

### 🛠️ Kurulum Adımları

#### ☕ Java & Maven
- Java 21 JDK ve Maven kurulumu
- Sürüm kontrolü ve doğrulama

#### 🐳 Docker Setup
- Docker engine kurulumu ve konfigürasyonu
- Docker Hub authentication
- User permissions ve grup ayarları

#### 🧹 Maintenance Scripts
- Disk cleanup otomasyonu
- Docker image ve container temizliği
- Volume management

### Makine 3: SonarQube Server

#### AWS EC2 Instance (t4g.medium)
1. **Instance Type**: `t4g.medium` (2 vCPU, 4GB RAM, ARM64)
2. **AMI**: Ubuntu Server 22.04 LTS (ARM64)
3. **Storage**: 15GB GP3 EBS
4. **Network**: VPC with internet gateway

### 🛠️ Kurulum Adımları

#### ☕ Java 11 Setup
- Java 11 JDK kurulumu (SonarQube requirement)
- Sürüm kontrolü ve doğrulama

#### 🐘 PostgreSQL Database
- PostgreSQL server kurulumu ve konfigürasyonu
- SonarQube database ve user oluşturma
- Database permissions ve access ayarları

#### 🔍 SonarQube Installation
- SonarQube binary indirme ve kurulum
- File permissions ve ownership ayarları
- SonarQube service başlatma ve konfigürasyon

### Makine 4: AWS EKS Server

#### AWS EC2 Instance (t4g.small)
1. **Instance Type**: `t4g.small` (2 vCPU, 2GB RAM, ARM64)
2. **AMI**: Ubuntu Server 22.04 LTS (ARM64)
3. **Storage**: 15GB GP3 EBS
4. **Network**: VPC with internet gateway

### 🛠️ Kurulum Adımları

#### 🏷️ System Configuration
- Hostname güncelleme ve system reboot
- System preparation ve network ayarları

#### ☁️ AWS Tools Installation
- AWS CLI kurulumu ve konfigürasyonu
- AWS credentials setup ve validation

#### ⚙️ Kubernetes Tools
- kubectl client kurulumu
- eksctl cluster management tool kurulumu
- Version kontrolü ve doğrulama

#### 🚀 EKS Cluster Setup
- AWS credentials konfigürasyonu
- EKS cluster oluşturma (my-workspace-cluster)
- Node group konfigürasyonu

#### 🔄 ArgoCD Deployment
- ArgoCD namespace ve deployment
- ArgoCD CLI kurulumu
- LoadBalancer konfigürasyonu ve admin access

## 🚀 Application Deployment

### Local Development

### 📦 Project Setup
- Git repository cloning ve directory navigation
- Maven build process ve JAR file validation
- Application startup ve local testing

### Docker ile Çalıştırma

### 🐳 Docker Operations
- Docker image building ve validation
- Container deployment ve port mapping
- Application testing ve health check

### Kubernetes ile Deployment

### ⚙️ Kubernetes Deployment
- Deployment YAML validation ve application
- Service configuration ve networking
- Pod status monitoring ve health checks
- Port forwarding ve application access

## 🌐 API Endpoints

| Endpoint | Method | Açıklama |
|----------|--------|----------|
| `/` | GET | Ana sayfa - Hoş geldin mesajı |
| `/info` | GET | Uygulama bilgileri |
| `/about` | GET | Hakkında bilgisi |

### Örnek Response
```json
{
  "message": "Version3 Hi Hello: 2024-01-15T10:30:45.123",
  "timestamp": "2024-01-15T10:30:45.123"
}
```

## ⚙️ Konfigürasyon

### Application Properties
```properties
spring.application.name=aws-pipeline
server.port=8080
```

### Docker Configuration
```dockerfile
FROM openjdk:21
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} devops-application.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "devops-application.jar"]
```

### Kubernetes Configuration
```yaml
# Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: devops-application-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: devops-application
  template:
    spec:
      containers:
      - name: devops-application
        image: onurguler18/devops-application:latest
        ports:
        - containerPort: 8080
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"

# Service
apiVersion: v1
kind: Service
metadata:
  name: devops-application-service
spec:
  selector:
    app: devops-application
  ports:
  - port: 9090
    targetPort: 8080
  type: NodePort
```

## 🔧 Jenkins Konfigürasyonu ve Bağlantıları

### 1. Jenkins Master İlk Kurulum

#### Jenkins'e İlk Giriş
1. **Jenkins Master makinesinde** terminal aç
2. **Admin şifresini al**:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
3. **Web tarayıcıda** `http://JENKINS_IP:8080` adresine git
4. **Admin şifresini** yapıştır ve "Continue" tıkla
5. **"Install suggested plugins"** seç
6. **Admin kullanıcısı oluştur**:
   - Username: `admin`
   - Password: `admin123` (güçlü şifre koy)
   - Full name: `Jenkins Admin`
   - Email: `admin@company.com`

#### Jenkins Plugin Kurulumu
1. **Manage Jenkins** → **Manage Plugins** → **Available** tab
2. **Arama kutusuna** plugin adını yaz ve **Install** tıkla:
   - `Docker Pipeline`
   - `Kubernetes CLI`
   - `SonarQube Scanner`
   - `Trivy Security Scanner`
   - `Git`
   - `Maven Integration`
   - `Blue Ocean` (opsiyonel)
3. **"Download now and install after restart"** seç
4. **Jenkins'i yeniden başlat**

### 2. Jenkins Credentials Yönetimi

#### Credentials Oluşturma
1. **Manage Jenkins** → **Manage Credentials** → **System** → **Global credentials**
2. **"Add Credentials"** tıkla

#### DockerHub Credentials
1. **Kind**: `Username with password`
2. **Scope**: `Global`
3. **Username**: `onurguler18`
4. **Password**: `DOCKER_HUB_TOKEN` (Personal Access Token)
5. **ID**: `dockerhub`
6. **Description**: `DockerHub credentials for image push`

#### SonarQube Token
1. **SonarQube'da token oluştur**:
   - `http://SONARQUBE_IP:9000` → **Administration** → **Security** → **Users**
   - **Tokens** → **Generate Token** → `jenkins-token`
2. **Jenkins'te credentials oluştur**:
   - **Kind**: `Secret text`
   - **Secret**: `SONARQUBE_TOKEN`
   - **ID**: `jenkins-sonar-token`
   - **Description**: `SonarQube authentication token`

#### Kubernetes Kubeconfig
1. **EKS makinesinde** kubeconfig dosyasını al:
   ```bash
   cat ~/.kube/config
   ```
2. **Jenkins'te credentials oluştur**:
   - **Kind**: `Secret file`
   - **File**: `kubeconfig` dosyasını upload et
   - **ID**: `kubernetes`
   - **Description**: `Kubernetes cluster access`

#### Jenkins API Token
1. **Jenkins Dashboard** → **Admin** → **Configure** → **API Token**
2. **"Add new Token"** → **Generate** → Token'ı kopyala
3. **Credentials oluştur**:
   - **Kind**: `Secret text`
   - **Secret**: `JENKINS_API_TOKEN`
   - **ID**: `JENKINS_API_TOKEN`
   - **Description**: `Jenkins API token for GitOps`

#### GitHub Token
1. **GitHub'da** → **Settings** → **Developer settings** → **Personal access tokens**
2. **"Generate new token"** → **"Generate new token (classic)"**
3. **Scopes**: `repo`, `admin:repo_hook`, `read:user`
4. **Token'ı kopyala**
5. **Jenkins'te credentials oluştur**:
   - **Kind**: `Secret text`
   - **Secret**: `GITHUB_TOKEN`
   - **ID**: `github-token`
   - **Description**: `GitHub access token`

### 3. Jenkins Agent Bağlantısı

#### Agent Makinesi Hazırlığı
1. **Agent makinesinde** Java 21, Maven, Docker kurulu olmalı
2. **SSH key** oluştur:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "jenkins-agent"
   ```
3. **Public key'i** Jenkins Master'a ekle:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

#### Jenkins Master'da Agent Ekleme
1. **Manage Jenkins** → **Manage Nodes** → **New Node**
2. **Node name**: `My-Jenkins-Agent`
3. **Type**: `Permanent Agent`
4. **Node properties**:
   - **Number of executors**: `4` (t4g.xlarge için optimize edilmiş)
   - **Remote root directory**: `/home/jenkins`
   - **Labels**: `My-Jenkins-Agent`
   - **Usage**: `Only build jobs with label expressions matching this node`
5. **Launch method**: `Launch agents via SSH`
6. **Host**: `AGENT_IP`
7. **Credentials**: SSH key credentials oluştur
8. **Host Key Verification Strategy**: `Non verifying Verification Strategy`

#### Agent Bağlantı Testi
1. **"Save"** tıkla
2. **Agent'ın "Online"** olduğunu kontrol et
3. **"Log"** tıkla ve bağlantı log'larını kontrol et

### 4. Jenkins Global Tool Configuration

#### Maven Konfigürasyonu
1. **Manage Jenkins** → **Global Tool Configuration**
2. **Maven** bölümünde **"Add Maven"** tıkla
3. **Name**: `Maven3`
4. **Install automatically** işaretle
5. **Version**: `3.9.0` (en son sürüm)

#### JDK Konfigürasyonu
1. **JDK** bölümünde **"Add JDK"** tıkla
2. **Name**: `Java21`
3. **Install automatically** işaretle
4. **Version**: `21` (en son sürüm)

### 5. Jenkins Job Oluşturma

#### Pipeline Job Oluşturma
1. **New Item** → **Pipeline** → **OK**
2. **Job name**: `aws-pipeline`
3. **Description**: `DevOps Pipeline for AWS EKS Deployment`

#### Pipeline Konfigürasyonu
1. **Pipeline** bölümünde:
   - **Definition**: `Pipeline script from SCM`
   - **SCM**: `Git`
   - **Repository URL**: `https://github.com/onurguler/aws-pipeline.git`
   - **Branches**: `*/main`
   - **Script Path**: `Jenkinsfile`

#### Build Triggers
1. **GitHub hook trigger for GITScm polling** işaretle
2. **Poll SCM** işaretle
3. **Schedule**: `H/5 * * * *` (5 dakikada bir kontrol)

### 6. SonarQube Server Konfigürasyonu

#### SonarQube'da Proje Oluşturma
1. **SonarQube'a giriş yap**: `http://SONARQUBE_IP:9000`
2. **Admin** → **Projects** → **Create Project**
3. **Project key**: `aws-pipeline`
4. **Display name**: `AWS Pipeline Project`
5. **Main branch**: `main`

#### SonarQube Quality Gate
1. **Quality Gates** → **Create**
2. **Name**: `AWS Pipeline Gate`
3. **Conditions**:
   - **Coverage**: `> 80%`
   - **Duplicated Lines**: `< 3%`
   - **Maintainability Rating**: `A`
   - **Reliability Rating**: `A`
   - **Security Rating**: `A`

### 7. ArgoCD Konfigürasyonu

#### ArgoCD'ye Giriş
1. **ArgoCD URL'ini al**: `kubectl get svc -n argocd`
2. **LoadBalancer IP** ile `https://ARGOCD_IP` adresine git
3. **Username**: `admin`
4. **Password**: `kubectl get secret argocd-initial-admin-secret -n argocd -o yaml`

#### ArgoCD Application Oluşturma
1. **Applications** → **New App**
2. **Application Name**: `devops-application`
3. **Project**: `default`
4. **Sync Policy**: `Automatic`
5. **Repository URL**: `https://github.com/onurguler/aws-pipeline.git`
6. **Path**: `.`
7. **Cluster**: `https://kubernetes.default.svc`
8. **Namespace**: `default`

### 8. Pipeline Test ve Doğrulama

#### İlk Build Testi
1. **Jenkins'te** `aws-pipeline` job'ını bul
2. **"Build Now"** tıkla
3. **Console Output**'u takip et
4. **Her stage'in** başarılı olduğunu kontrol et

#### Bağlantı Testleri
1. **Docker Hub**: Image push edildi mi?
2. **SonarQube**: Quality gate geçti mi?
3. **Kubernetes**: Pod'lar oluştu mu?
4. **ArgoCD**: Application sync oldu mu?

#### Monitoring
1. **Jenkins Dashboard**: Build durumları
2. **SonarQube Dashboard**: Code quality
3. **ArgoCD Dashboard**: Application status
4. **Kubernetes Dashboard**: Pod ve service durumları


## 📊 Monitoring ve Logging

### Jenkins Monitoring

#### Build Durumu Kontrolü
1. **Jenkins API** ile build durumunu kontrol et:
   - `curl -u admin:JENKINS_API_TOKEN http://JENKINS_IP:8080/job/aws-pipeline/lastBuild/api/json`
2. **Build log'larını** görüntüle:
   - Jenkins web arayüzünde **"Console Output"** tıkla

#### Jenkins Log'ları
1. **Jenkins log dosyasını** takip et:
   - `tail -f /var/log/jenkins/jenkins.log`
2. **Log seviyesini** ayarla:
   - Jenkins **"Manage Jenkins"** → **"System Log"**

#### Disk Kullanımı
1. **Jenkins disk kullanımını** kontrol et:
   - `df -h /var/lib/jenkins`
2. **Build artifact'larını** temizle:
   - Jenkins **"Manage Jenkins"** → **"Disk Usage"**

### Kubernetes Monitoring

#### Pod Durumu
1. **Pod'ları listele**:
   - `kubectl get pods -l app=devops-application`
2. **Pod detaylarını** görüntüle:
   - `kubectl describe pod <pod-name>`
3. **Pod log'larını** görüntüle:
   - `kubectl logs <pod-name>`

#### Service Durumu
1. **Service'leri listele**:
   - `kubectl get svc devops-application-service`
2. **Service detaylarını** görüntüle:
   - `kubectl describe svc devops-application-service`

#### Cluster Durumu
1. **Node'ları listele**:
   - `kubectl get nodes`
2. **Cluster bilgilerini** görüntüle:
   - `kubectl cluster-info`

### Application Logs

#### Real-time Log Takibi
1. **Deployment log'larını** takip et:
   - `kubectl logs -f deployment/devops-application-deployment`
2. **Belirli pod'un log'larını** görüntüle:
   - `kubectl logs <pod-name> --previous`

#### Log Kaydetme
1. **Log'ları dosyaya** kaydet:
   - `kubectl logs deployment/devops-application-deployment > app-logs.txt`
2. **Log rotation** ayarla:
   - Kubernetes **ConfigMap** ile log rotation yapılandır

### SonarQube Monitoring

#### SonarQube Durumu
1. **SonarQube API** ile durumu kontrol et:
   - `curl http://SONARQUBE_IP:9000/api/system/status`
2. **Web arayüzünde** durumu kontrol et:
   - `http://SONARQUBE_IP:9000`

#### SonarQube Log'ları
1. **SonarQube log dosyasını** takip et:
   - `tail -f /opt/sonarqube/logs/sonar.log`
2. **Log seviyesini** ayarla:
   - SonarQube **"Administration"** → **"System"** → **"Logs"**

#### Veritabanı Durumu
1. **PostgreSQL bağlantısını** test et:
   - `sudo -u postgres psql -c "SELECT * FROM pg_stat_activity WHERE datname='sonarqube';"`
2. **Veritabanı boyutunu** kontrol et:
   - `sudo -u postgres psql -c "SELECT pg_size_pretty(pg_database_size('sonarqube'));"`

### ArgoCD Monitoring

#### ArgoCD Uygulamaları
1. **Uygulamaları listele**:
   - `argocd app list`
2. **Uygulama durumunu** kontrol et:
   - `argocd app get devops-application`

#### ArgoCD Sync
1. **Sync durumunu** kontrol et:
   - `argocd app sync devops-application`
2. **Sync geçmişini** görüntüle:
   - ArgoCD web arayüzünde **"Application History"**

#### ArgoCD Log'ları
1. **ArgoCD server log'larını** görüntüle:
   - `kubectl logs -n argocd deployment/argocd-server`
2. **ArgoCD application controller log'larını** görüntüle:
   - `kubectl logs -n argocd deployment/argocd-application-controller`

## 🔒 Güvenlik

### Security Scanning
- **Trivy**: Container image güvenlik taraması
- **SonarQube**: Kod kalitesi ve güvenlik analizi
- **Docker**: Multi-stage build ile güvenli image oluşturma

### Best Practices
- Container image'ları güncel base image'larla oluşturma
- Resource limits tanımlama
- Security scanning'i pipeline'a entegre etme
- Secrets management

## 🚀 Deployment Stratejisi

### Rolling Update
```bash
# Yeni versiyonu deploy et
kubectl set image deployment/devops-application-deployment devops-application=onurguler18/devops-application:1.0.123

# Rollout durumunu kontrol et
kubectl rollout status deployment/devops-application-deployment

# Rollback (gerekirse)
kubectl rollout undo deployment/devops-application-deployment
```

## 📈 Performans ve Scaling

### Resource Management (t4g.xlarge optimized)
```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"
```

### Auto Scaling
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: devops-application-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: devops-application-deployment
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

## 🛠️ Troubleshooting

### Jenkins Issues

#### 1. Jenkins Build Fails
```bash
# Jenkins log'larını kontrol et
tail -f /var/log/jenkins/jenkins.log

# Build log'larını görüntüle
curl -u admin:JENKINS_API_TOKEN http://JENKINS_IP:8080/job/aws-pipeline/lastBuild/consoleText

# Jenkins servisini yeniden başlat
sudo systemctl restart jenkins
sudo systemctl status jenkins
```

#### 2. Agent Connection Issues
```bash
# Agent durumunu kontrol et
curl -u admin:JENKINS_API_TOKEN http://JENKINS_IP:8080/computer/api/json

# SSH bağlantısını test et
ssh jenkins@AGENT_IP

# Agent makinesinde Jenkins servisini kontrol et
sudo systemctl status jenkins-agent
```

### Kubernetes Issues

#### 1. Pod CrashLoopBackOff
```bash
# Pod detaylarını kontrol et
kubectl describe pod <pod-name>

# Log'ları incele
kubectl logs <pod-name>

# Pod'u yeniden başlat
kubectl delete pod <pod-name>
```

#### 2. Service Connection Issues
```bash
# Service endpoint'lerini kontrol et
kubectl get endpoints devops-application-service

# Port forwarding ile test et
kubectl port-forward svc/devops-application-service 8080:9090

# Service detaylarını görüntüle
kubectl describe svc devops-application-service
```

#### 3. Image Pull Issues
```bash
# Image'ın mevcut olup olmadığını kontrol et
kubectl get events --sort-by=.metadata.creationTimestamp

# Docker Hub'dan image'ı manuel çek
docker pull onurguler18/devops-application:latest

# Image'ı Kubernetes'e push et
docker tag onurguler18/devops-application:latest localhost:5000/devops-application:latest
```

### Docker Issues

#### 1. Docker Build Fails
```bash
# Docker daemon durumunu kontrol et
docker info

# Docker log'larını görüntüle
journalctl -u docker.service

# Docker servisini yeniden başlat
sudo systemctl restart docker
```

#### 2. Docker Hub Push Fails
```bash
# Docker Hub login durumunu kontrol et
docker login

# Image'ı manuel push et
docker push onurguler18/devops-application:latest

# Docker Hub token'ını yenile
docker logout
docker login -u onurguler18 -p NEW_TOKEN
```

#### 3. Disk Space Issues
```bash
# Disk kullanımını kontrol et
df -h

# Eski image'ları temizle
docker rmi $(docker images --format '{{.Repository}}:{{.Tag}}' | grep 'devops-03-pipeline-aws')
docker container rm -f $(docker container ls -aq)
docker volume prune -f
docker system prune -a
```

### SonarQube Issues

#### 1. SonarQube Won't Start
```bash
# SonarQube log'larını kontrol et
tail -f /opt/sonarqube/logs/sonar.log

# Java sürümünü kontrol et
java -version

# PostgreSQL bağlantısını test et
sudo -u postgres psql -c "SELECT version();"

# SonarQube'u yeniden başlat
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh restart
```

#### 2. Quality Gate Fails
```bash
# SonarQube proje durumunu kontrol et
curl http://SONARQUBE_IP:9000/api/qualitygates/project_status?projectKey=aws-pipeline

# SonarQube'da proje ayarlarını kontrol et
# http://SONARQUBE_IP:9000/project/quality_gate?id=aws-pipeline
```

### ArgoCD Issues

#### 1. ArgoCD Sync Fails
```bash
# ArgoCD uygulama durumunu kontrol et
argocd app get devops-application

# ArgoCD log'larını görüntüle
kubectl logs -n argocd deployment/argocd-server

# Manuel sync yap
argocd app sync devops-application --force
```

#### 2. ArgoCD Connection Issues
```bash
# ArgoCD servis durumunu kontrol et
kubectl get svc -n argocd

# ArgoCD pod'larını kontrol et
kubectl get pods -n argocd

# ArgoCD'yi yeniden başlat
kubectl rollout restart deployment/argocd-server -n argocd
```

### EKS Issues

#### 1. EKS Cluster Issues
```bash
# Cluster durumunu kontrol et
eksctl get cluster --name my-workspace-cluster --region us-east-2

# Node durumunu kontrol et
kubectl get nodes

# Cluster'ı yeniden oluştur
eksctl delete cluster --name my-workspace-cluster --region us-east-2
eksctl create cluster --name my-workspace-cluster --region us-east-2 --node-type t4g.medium --nodes 2
```

#### 2. AWS CLI Issues
```bash
# AWS CLI konfigürasyonunu kontrol et
aws configure list

# AWS credentials'ı yenile
aws configure

# AWS region'ı ayarla
export AWS_DEFAULT_REGION=us-east-2
```

## 📚 Kaynaklar

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [SonarQube Documentation](https://docs.sonarqube.org/)
- [Trivy Documentation](https://aquasecurity.github.io/trivy/)

## 🤝 Katkıda Bulunma

1. Fork yapın
2. Feature branch oluşturun (`git checkout -b feature/amazing-feature`)
3. Değişikliklerinizi commit edin (`git commit -m 'Add amazing feature'`)
4. Branch'inizi push edin (`git push origin feature/amazing-feature`)
5. Pull Request oluşturun

## 📄 Lisans

Bu proje MIT lisansı altında lisanslanmıştır. Detaylar için [LICENSE](LICENSE) dosyasına bakın.

## 👨‍💻 Geliştirici

**Onur Güler**
- GitHub: [@onurglr](https://github.com/onurglr)
- LinkedIn: [Onur Güler](https://linkedin.com/in/onurguler-dev)

---

## 🎯 Proje Hedefleri

Bu proje aşağıdaki DevOps hedeflerini gerçekleştirmek için tasarlanmıştır:

- ✅ **Otomatik CI/CD Pipeline**
- ✅ **Container Orchestration**
- ✅ **Code Quality Management**
- ✅ **Security Scanning**
- ✅ **Infrastructure as Code**
- ✅ **Monitoring & Logging**
- ✅ **Scalable Architecture**

---

*Son güncelleme: 2024-01-15*
