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

### 🔧 Jenkins Configuration

#### 🚀 Initial Setup
- Admin password retrieval ve web interface access
- Plugin installation (Docker, Kubernetes, SonarQube, Trivy, Git, Maven)
- Admin user creation ve security configuration
- Jenkins service restart ve validation

#### 🔐 Credentials Management
- DockerHub authentication (Personal Access Token)
- SonarQube token generation ve configuration
- Kubernetes kubeconfig file upload
- Jenkins API token creation
- GitHub personal access token setup

#### 🤖 Agent Connection
- SSH key generation ve Master-Agent authentication
- Node configuration (4 executors for t4g.xlarge optimization)
- Agent connection testing ve status validation

#### ⚙️ Global Tools Setup
- Maven 3.9.0 automatic installation configuration
- Java 21 JDK automatic installation setup
- Tool validation ve version verification

#### 📋 Pipeline Job Creation
- New pipeline job creation (aws-pipeline)
- SCM configuration (Git repository integration)
- Build triggers setup (GitHub webhook ve SCM polling)

#### 🔍 SonarQube Configuration
- Project creation (aws-pipeline project setup)
- Quality Gate configuration (Coverage >80%, Security Rating A)
- Project validation ve integration testing

#### 🔄 ArgoCD Setup
- ArgoCD web interface access ve authentication
- Application creation (devops-application)
- Automatic sync policy configuration
- Repository ve cluster integration

#### ✅ Pipeline Validation
- Initial build testing ve console output monitoring
- Integration validation (Docker Hub, SonarQube, Kubernetes, ArgoCD)
- Dashboard monitoring ve status verification


## 📊 Monitoring ve Logging

### 🔧 Jenkins Monitoring
- Build status API integration ve console output monitoring
- Jenkins log tracking ve system log configuration
- Disk usage monitoring ve build artifact cleanup

### ⚙️ Kubernetes Monitoring
- Pod status monitoring ve detailed pod inspection
- Service status tracking ve cluster health checks
- Real-time log monitoring ve log rotation setup

### 🔍 SonarQube Monitoring
- System status API integration ve web interface monitoring
- SonarQube log tracking ve administration configuration
- PostgreSQL database status ve size monitoring

### 🔄 ArgoCD Monitoring
- Application status tracking ve sync monitoring
- ArgoCD server ve application controller log monitoring
- Application history ve sync validation

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

### 🔧 Jenkins Issues
- Build failure diagnostics ve log analysis
- Agent connection troubleshooting ve SSH validation
- Service restart procedures ve status verification

### ⚙️ Kubernetes Issues
- Pod crash diagnostics ve restart procedures
- Service connection troubleshooting ve endpoint validation
- Image pull issues ve Docker Hub connectivity

### 🐳 Docker Issues
- Build failure diagnostics ve daemon status checks
- Docker Hub push issues ve authentication troubleshooting
- Disk space management ve cleanup procedures

### 🔍 SonarQube Issues
- Service startup issues ve log analysis
- Quality gate failure troubleshooting ve project status checks
- PostgreSQL connectivity ve database validation

### 🔄 ArgoCD Issues
- Sync failure diagnostics ve manual sync procedures
- Connection issues ve service restart procedures
- Application status validation ve troubleshooting

### ☁️ EKS Issues
- Cluster status monitoring ve recreation procedures
- AWS CLI configuration ve credentials management
- Node status validation ve cluster health checks

## 📚 Kaynaklar

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [SonarQube Documentation](https://docs.sonarqube.org/)
- [Trivy Documentation](https://aquasecurity.github.io/trivy/)

## 🤝 Katkıda Bulunma

- Fork yapın ve feature branch oluşturun
- Değişikliklerinizi commit edin ve push edin
- Pull Request oluşturun

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

