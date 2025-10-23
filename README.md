# 🚀 AWS DevOps Pipeline Projesi

## 📋 Proje Özeti
Bu proje, modern DevOps uygulamalarını kullanarak Spring Boot uygulamasının tam otomatik CI/CD sürecini içerir. Jenkins, Docker, Kubernetes, SonarQube, Trivy ve AWS EKS teknolojileri kullanılarak geliştirilmiştir.

## 🛠️ Teknoloji Yığını

| Teknoloji | Versiyon | Açıklama |
|-----------|----------|----------|
| **Java** | 21 | Backend programlama dili |
| **Spring Boot** | 3.5.5 | Web çatısı |
| **Maven** | 3.9+ | Derleme aracı |
| **Docker** | En Son | Kapsayıcılaştırma |
| **Kubernetes** | 1.30+ | Kapsayıcı orkestrasyonu |
| **Jenkins** | 2.400+ | CI/CD otomasyonu |
| **SonarQube** | 9.0+ | Kod kalitesi analizi |
| **Trivy** | En Son | Güvenlik taraması |
| **AWS EKS** | 1.30+ | Yönetilen Kubernetes servisi |

## 🏗️ Sistem Mimarisi

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Geliştirici   │    │     GitHub      │    │    Jenkins      │
│                 │    │   Depo          │    │     Sunucu      │
│  Kod Gönderimi  │───▶│                 │───▶│                 │
│                 │    │  Webhook Tetikleme│   │  Süreç Başlatma │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                       │
                                                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Docker Hub    │    │   SonarQube     │    │     Trivy       │
│                 │    │                 │    │                 │
│  Görüntü Depolama│◀───│  Kod Kalitesi   │◀───│ Güvenlik Tarama │
│                 │    │   Analizi       │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │
         ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   AWS EKS       │    │   Kubernetes    │    │   Uygulama      │
│                 │    │                 │    │                 │
│  Küme           │───▶│   Dağıtım       │───▶│   Üretim        │
│  Yönetimi       │    │   & Servis      │    │   Ortamı        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🔄 DevOps Pipeline Detayları

### 📋 Pipeline Aşamaları

#### 1. **Kaynak Kod Yönetimi (SCM)**
- **GitHub Depo**: Kod değişikliklerinin takibi
- **Dal Stratejisi**: Ana dal'dan otomatik tetikleme
- **Webhook Entegrasyonu**: Gerçek zamanlı derleme tetikleme
- **Sürüm Kontrolü**: Git etiketleri ile sürüm yönetimi

#### 2. **Test Aşaması**
- **Birim Testleri**: Maven test çatısı ile otomatik testler
- **Test Kapsamı**: Kod kapsamı analizi ve raporlama
- **Kalite Metrikleri**: Test başarı oranı ve performans metrikleri
- **Test Sonuçları**: Jenkins kontrol panelinde test sonuçları görüntüleme

#### 3. **Derleme Aşaması**
- **Maven Derleme**: Temiz kurulum ile proje derleme
- **Bağımlılık Yönetimi**: Bağımlılık çözümleme ve kontrolü
- **Eser Oluşturma**: JAR dosyası oluşturma ve doğrulama
- **Derleme Optimizasyonu**: Derleme süre optimizasyonu ve önbellek kullanımı

#### 4. **Kod Kalitesi Analizi**
- **SonarQube Entegrasyonu**: Kod kalitesi ve güvenlik analizi
- **Kod Kokusu Tespiti**: Kod kokularının tespiti ve düzeltme önerileri
- **Güvenlik Taraması**: Güvenlik açıklarının tespiti
- **Kalite Kapısı**: Kalite kriterlerinin karşılanması kontrolü

#### 5. **Kapsayıcılaştırma**
- **Docker Derleme**: Çok aşamalı derleme ile optimize edilmiş görüntü oluşturma
- **Görüntü Etiketleme**: Sürüm numarası ile görüntü etiketleme
- **Kayıt Defteri Gönderimi**: DockerHub'a güvenli görüntü yükleme
- **Görüntü Optimizasyonu**: Boyut optimizasyonu ve güvenlik taraması

#### 6. **Güvenlik Taraması**
- **Trivy Entegrasyonu**: Kapsayıcı görüntü güvenlik taraması
- **Güvenlik Açığı Değerlendirmesi**: YÜKSEK ve KRİTİK seviye güvenlik açıklarının kontrolü
- **Güvenlik Raporları**: Güvenlik raporlarının oluşturulması
- **Uyumluluk Kontrolü**: Güvenlik standartlarına uygunluk kontrolü

#### 7. **Kubernetes Dağıtımı**
- **EKS Entegrasyonu**: AWS EKS kümesine otomatik dağıtım
- **Servis Yapılandırması**: Yük dengeleyici ve servis yapılandırması
- **Sağlık Kontrolleri**: Pod sağlık kontrolü ve hazırlık sondası
- **Yuvarlanma Güncellemesi**: Sıfır kesinti süresi ile güncelleme

#### 8. **İzleme ve Günlük Tutma**
- **Uygulama İzleme**: Uygulama performans ve sağlık takibi
- **Günlük Toplama**: Merkezi günlük toplama ve analiz
- **Uyarı Sistemi**: Kritik durumlar için otomatik uyarı sistemi
- **Kontrol Paneli**: Gerçek zamanlı izleme kontrol paneli

#### 9. **Temizlik ve Optimizasyon**
- **Kaynak Temizliği**: Eski görüntülerin ve kapsayıcıların temizlenmesi
- **Disk Optimizasyonu**: Disk alanı optimizasyonu
- **Performans Ayarları**: Sistem performans ayarları
- **Maliyet Optimizasyonu**: AWS maliyet optimizasyonu

## 📁 Proje Yapısı

```
aws-pipeline/
├── src/
│   ├── main/
│   │   ├── java/com/onurguler/
│   │   │   ├── AppMain.java              # Spring Boot ana sınıf
│   │   │   └── controller/
│   │   │       └── DevOpsController.java # REST API uç noktaları
│   │   └── resources/
│   │       └── application.properties    # Uygulama yapılandırması
│   └── test/                             # Test sınıfları
├── target/                               # Derleme eserleri
├── Dockerfile                           # Docker yapılandırması
├── deployment.yaml                      # K8s dağıtımı
├── service.yaml                         # K8s servisi
├── Jenkinsfile                          # CI/CD süreci
├── pom.xml                             # Maven yapılandırması
└── README.md                           # Proje dokümantasyonu
```

## 🏗️ Altyapı Kurulumu

### AWS Örnek Türleri (t4g.xlarge referans alınarak)

| Makine | Örnek Türü | vCPU | RAM | Depolama | Açıklama |
|--------|---------------|------|-----|---------|----------|
| Jenkins Ana | t4g.xlarge | 4 | 16GB | 15GB | Ana CI/CD sunucusu |
| Jenkins Ajan | t4g.large | 2 | 8GB | 15GB | Derleme işlemleri için |
| SonarQube | t4g.medium | 2 | 4GB | 15GB | Kod kalitesi analizi |
| EKS Bootstrap | t4g.small | 2 | 2GB | 15GB | Küme yönetimi |
| EKS Düğümleri | t4g.medium | 2 | 4GB | 15GB | Uygulama iş yükleri |

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

### 📦 Temel Dağıtım
- Git repository cloning ve Maven build process
- Docker image building ve container deployment
- Kubernetes deployment ve service configuration

## 🌐 API Uç Noktaları

| Endpoint | Method | Açıklama |
|----------|--------|----------|
| `/` | GET | Ana sayfa - Hoş geldin mesajı |
| `/info` | GET | Uygulama bilgileri |
| `/about` | GET | Hakkında bilgisi |

### Örnek Yanıt
```json
{
  "message": "Version3 Hi Hello: 2024-01-15T10:30:45.123",
  "timestamp": "2024-01-15T10:30:45.123"
}
```

## ⚙️ DevOps Konfigürasyon Detayları

### 🔧 Jenkins Pipeline Konfigürasyonu
- **Pipeline Script**: Declarative pipeline syntax ile CI/CD otomasyonu
- **Build Triggers**: GitHub webhook ve SCM polling konfigürasyonu
- **Environment Variables**: Build environment ve credential yönetimi
- **Parallel Execution**: Multi-stage pipeline ile paralel build execution

### 🐳 Docker Konfigürasyonu
- **Multi-stage Build**: Production-ready image oluşturma
- **Security Scanning**: Container güvenlik taraması ve vulnerability check
- **Image Optimization**: Layer caching ve boyut optimizasyonu
- **Registry Integration**: DockerHub authentication ve push automation

### ⚙️ Kubernetes Konfigürasyonu
- **Deployment Strategy**: Rolling update ve zero-downtime deployment
- **Resource Management**: CPU ve memory limits ile resource optimization
- **Health Checks**: Liveness ve readiness probe konfigürasyonu
- **Service Mesh**: Load balancing ve service discovery

### 🔍 SonarQube Konfigürasyonu
- **Quality Gates**: Kod kalitesi kriterleri ve threshold ayarları
- **Code Coverage**: Test coverage requirements ve reporting
- **Security Rules**: Güvenlik kuralları ve vulnerability detection
- **Integration**: Jenkins pipeline ile otomatik quality gate kontrolü

### 🔄 ArgoCD Konfigürasyonu
- **GitOps Workflow**: Git-based deployment ve configuration management
- **Sync Policies**: Otomatik sync ve manual approval workflows
- **Application Monitoring**: Deployment status ve health monitoring
- **Rollback Capabilities**: Hızlı rollback ve version management

### 📊 Monitoring Konfigürasyonu
- **Metrics Collection**: Application ve infrastructure metrics
- **Log Aggregation**: Centralized logging ve log analysis
- **Alerting Rules**: Threshold-based alerting ve notification
- **Dashboard Configuration**: Real-time monitoring ve visualization

## 🔧 Jenkins Konfigürasyonu ve Bağlantıları

### 🔧 Jenkins Konfigürasyonu

#### 🚀 İlk Kurulum
- Admin password retrieval ve web interface access
- Plugin installation (Docker, Kubernetes, SonarQube, Trivy, Git, Maven)
- Admin user creation ve security configuration
- Jenkins service restart ve validation

#### 🔐 Kimlik Bilgileri Yönetimi
- DockerHub authentication (Personal Access Token)
- SonarQube token generation ve configuration
- Kubernetes kubeconfig file upload
- Jenkins API token creation
- GitHub personal access token setup

#### 🤖 Ajan Bağlantısı
- SSH key generation ve Master-Agent authentication
- Node configuration (4 executors for t4g.xlarge optimization)
- Agent connection testing ve status validation

#### ⚙️ Global Araçlar Kurulumu
- Maven 3.9.0 automatic installation configuration
- Java 21 JDK automatic installation setup
- Tool validation ve version verification

#### 📋 Pipeline İş Oluşturma
- New pipeline job creation (aws-pipeline)
- SCM configuration (Git repository integration)
- Build triggers setup (GitHub webhook ve SCM polling)

#### 🔍 SonarQube Konfigürasyonu
- Project creation (aws-pipeline project setup)
- Quality Gate configuration (Coverage >80%, Security Rating A)
- Project validation ve integration testing

#### 🔄 ArgoCD Kurulumu
- ArgoCD web interface access ve authentication
- Application creation (devops-application)
- Automatic sync policy configuration
- Repository ve cluster integration

#### ✅ Pipeline Doğrulama
- Initial build testing ve console output monitoring
- Integration validation (Docker Hub, SonarQube, Kubernetes, ArgoCD)
- Dashboard monitoring ve status verification


## 📊 İzleme ve Günlük Tutma

### 🔧 Jenkins İzleme
- Build status API integration ve console output monitoring
- Jenkins log tracking ve system log configuration
- Disk usage monitoring ve build artifact cleanup

### ⚙️ Kubernetes İzleme
- Pod status monitoring ve detailed pod inspection
- Service status tracking ve cluster health checks
- Real-time log monitoring ve log rotation setup

### 🔍 SonarQube İzleme
- System status API integration ve web interface monitoring
- SonarQube log tracking ve administration configuration
- PostgreSQL database status ve size monitoring

### 🔄 ArgoCD İzleme
- Application status tracking ve sync monitoring
- ArgoCD server ve application controller log monitoring
- Application history ve sync validation

## 🔒 Güvenlik

### Güvenlik Taraması
- **Trivy**: Container image güvenlik taraması
- **SonarQube**: Kod kalitesi ve güvenlik analizi
- **Docker**: Multi-stage build ile güvenli image oluşturma

### En İyi Uygulamalar
- Container image'ları güncel base image'larla oluşturma
- Resource limits tanımlama
- Security scanning'i pipeline'a entegre etme
- Secrets management

## 🚀 Dağıtım Stratejisi

### Rolling Update
```bash
# Yeni versiyonu deploy et
kubectl set image deployment/devops-application-deployment devops-application=onurguler18/devops-application:1.0.123

# Rollout durumunu kontrol et
kubectl rollout status deployment/devops-application-deployment

# Rollback (gerekirse)
kubectl rollout undo deployment/devops-application-deployment
```

## 📈 Performans ve Ölçeklendirme

### Kaynak Yönetimi (t4g.xlarge optimize edilmiş)
```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"
```

### Otomatik Ölçeklendirme
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

## 🛠️ Sorun Giderme

### 🔧 Jenkins Sorunları
- Build failure diagnostics ve log analysis
- Agent connection troubleshooting ve SSH validation
- Service restart procedures ve status verification

### ⚙️ Kubernetes Sorunları
- Pod crash diagnostics ve restart procedures
- Service connection troubleshooting ve endpoint validation
- Image pull issues ve Docker Hub connectivity

### 🐳 Docker Sorunları
- Build failure diagnostics ve daemon status checks
- Docker Hub push issues ve authentication troubleshooting
- Disk space management ve cleanup procedures

### 🔍 SonarQube Sorunları
- Service startup issues ve log analysis
- Quality gate failure troubleshooting ve project status checks
- PostgreSQL connectivity ve database validation

### 🔄 ArgoCD Sorunları
- Sync failure diagnostics ve manual sync procedures
- Connection issues ve service restart procedures
- Application status validation ve troubleshooting

### ☁️ EKS Sorunları
- Cluster status monitoring ve recreation procedures
- AWS CLI configuration ve credentials management
- Node status validation ve cluster health checks

## 📚 Kaynaklar

- [Spring Boot Dokümantasyonu](https://spring.io/projects/spring-boot)
- [Docker Dokümantasyonu](https://docs.docker.com/)
- [Kubernetes Dokümantasyonu](https://kubernetes.io/docs/)
- [Jenkins Dokümantasyonu](https://www.jenkins.io/doc/)
- [SonarQube Dokümantasyonu](https://docs.sonarqube.org/)
- [Trivy Dokümantasyonu](https://aquasecurity.github.io/trivy/)

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

