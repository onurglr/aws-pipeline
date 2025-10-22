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

## 🚀 Kurulum ve Çalıştırma

### Gereksinimler
- Java 21
- Maven 3.9+
- Docker
- Kubernetes cluster (EKS)
- Jenkins server

### Local Development
```bash
# Repository'yi klonla
git clone https://github.com/onurguler/aws-pipeline.git
cd aws-pipeline

# Maven ile build
mvn clean package

# Uygulamayı çalıştır
java -jar target/devops-application.jar
```

### Docker ile Çalıştırma
```bash
# Docker image oluştur
docker build -t devops-application .

# Container'ı çalıştır
docker run -p 8080:8080 devops-application
```

### Kubernetes ile Deployment
```bash
# Deployment oluştur
kubectl apply -f deployment.yaml

# Service oluştur
kubectl apply -f service.yaml

# Pod'ları kontrol et
kubectl get pods

# Service'i kontrol et
kubectl get services
```

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

## 🔧 Jenkins Pipeline Konfigürasyonu

### Environment Variables
```groovy
environment {
    APP_NAME = "devops-03-pipeline-aws"
    RELEASE = "1.0"
    DOCKER_USER = "onurguler18"
    DOCKER_LOGIN = 'dockerhub'
    IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
    IMAGE_TAG = "${RELEASE}.${BUILD_NUMBER}"
    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
}
```

### Required Jenkins Plugins
- Docker Pipeline
- Kubernetes CLI
- SonarQube Scanner
- Trivy Security Scanner

### Required Jenkins Credentials
- `dockerhub` - DockerHub credentials
- `jenkins-sonar-token` - SonarQube token
- `kubernetes` - Kubernetes kubeconfig
- `JENKINS_API_TOKEN` - Jenkins API token

## 📊 Monitoring ve Logging

### Health Checks
```bash
# Pod durumunu kontrol et
kubectl get pods -l app=devops-application

# Log'ları görüntüle
kubectl logs -l app=devops-application

# Service durumunu kontrol et
kubectl get svc devops-application-service
```

### Application Logs
```bash
# Real-time log takibi
kubectl logs -f deployment/devops-application-deployment
```

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

### Resource Management
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
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

### Common Issues

#### 1. Pod CrashLoopBackOff
```bash
# Pod detaylarını kontrol et
kubectl describe pod <pod-name>

# Log'ları incele
kubectl logs <pod-name>
```

#### 2. Service Connection Issues
```bash
# Service endpoint'lerini kontrol et
kubectl get endpoints devops-application-service

# Port forwarding ile test et
kubectl port-forward svc/devops-application-service 8080:9090
```

#### 3. Docker Build Issues
```bash
# Docker daemon durumunu kontrol et
docker info

# Image'ları temizle
docker system prune -a
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
- GitHub: [@onurguler](https://github.com/onurguler)
- LinkedIn: [Onur Güler](https://linkedin.com/in/onurguler)

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
