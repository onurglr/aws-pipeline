# Detaylı DevOps Araç Diyagramları

## 🚀 Jenkins Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "Jenkins Pipeline Stages"
        A[📥 GitHub Webhook] --> B[🔍 SCM Checkout]
        B --> C[🧪 Test Maven]
        C --> D[🔨 Build Maven]
        D --> E[🔍 SonarQube Analysis]
        E --> F{Quality Gate}
        F -->|Pass| G[🐳 Docker Build & Push]
        F -->|Fail| I[❌ Build Fail]
        
        G --> H[📦 DockerHub Registry]
        H --> J[🔒 Trivy Scan]
        J --> K{Security Check}
        K -->|Pass| L[🧹 Docker Cleanup]
        K -->|Fail| I
        
        L --> M[🔄 Trigger ArgoCD]
        M --> N[✅ Pipeline Complete]
    end
    
    subgraph "Jenkins Integration Points"
        P[GitHub Repository] --> A
        Q[SonarQube Server] --> E
        R[Docker Hub Registry] --> H
        S[Trivy Scanner] --> J
        T[ArgoCD API] --> M
    end
    
    style A fill:#fff3e0
    style F fill:#e8f5e8
    style K fill:#ffebee
    style I fill:#ffcdd2
    style N fill:#c8e6c9
```

## 🐳 Docker Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "Docker Build & Push Process"
        A[📥 Jenkins Trigger<br/>Quality Gate Pass] --> B[📋 Dockerfile Analysis]
        B --> C[🏗️ Multi-stage Build]
        
        subgraph "Build Stages"
            C1[📦 Base Image Pull]
            C2[📚 Dependency Installation]
            C3[🔨 Application Build]
            C4[🧹 Cleanup]
        end
        
        C --> C1
        C1 --> C2
        C2 --> C3
        C3 --> C4
        
        C4 --> D[🏷️ Image Tagging<br/>1.0.BUILD_NUMBER<br/>latest]
        D --> E[📤 DockerHub Push]
        E --> F[📦 Image in Registry]
        F --> G[🔒 Trivy Scan<br/>Security Check]
    end
    
    subgraph "Docker Integration"
        H[Jenkins Pipeline] --> A
        I[Docker Hub Registry] --> E
        J[Trivy Scanner] --> G
        K[Kubernetes Pull] --> F
    end
    
    style A fill:#fff3e0
    style E fill:#e8f5e8
    style G fill:#ffebee
    style F fill:#f1f8e9
```

## ⚙️ Kubernetes Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "Kubernetes Deployment Process"
        A[🔄 ArgoCD Sync] --> B[📋 Manifest Analysis<br/>deployment.yaml<br/>service.yaml]
        B --> C[🏷️ Image Pull<br/>DockerHub]
        C --> D[🔍 Resource Check]
        D --> E[📦 Pod Creation]
        
        subgraph "Pod Lifecycle"
            E1[🔄 Container Start]
            E2[💓 Health Check]
            E3[🚀 Readiness Probe]
            E4[✅ Running State]
        end
        
        E --> E1
        E1 --> E2
        E2 --> E3
        E3 --> E4
        
        E4 --> F[🌐 Service Creation<br/>LoadBalancer]
        F --> G[📊 Status Update]
        
        subgraph "Monitoring & Scaling"
            H[📈 Metrics Collection]
            I[🔄 Auto Scaling]
            J[🔄 Rolling Update]
            K[🔍 Health Monitoring]
        end
        
        G --> H
        H --> I
        H --> J
        H --> K
    end
    
    subgraph "Kubernetes Integration"
        L[ArgoCD GitOps] --> A
        M[Docker Hub] --> C
        N[Prometheus] --> H
        O[GitHub GitOps Repo] --> L
    end
    
    style A fill:#e0f2f1
    style E2 fill:#e8f5e8
    style E3 fill:#e8f5e8
    style E4 fill:#c8e6c9
```

## 🔍 SonarQube Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "SonarQube Analysis Process"
        A[📥 Jenkins Trigger] --> B[📋 Project Configuration]
        B --> C[📚 Code Download]
        C --> D[🔍 Static Analysis]
        
        subgraph "Analysis Components"
            D1[🐛 Bug Detection]
            D2[💨 Code Smell Detection]
            D3[🔒 Security Hotspot]
            D4[📊 Coverage Analysis]
            D5[📈 Duplication Check]
        end
        
        D --> D1
        D --> D2
        D --> D3
        D --> D4
        D --> D5
        
        D1 --> E[📊 Quality Metrics]
        D2 --> E
        D3 --> E
        D4 --> E
        D5 --> E
        
        E --> F{Quality Gate}
        F -->|Pass| G[✅ Build Continue]
        F -->|Fail| H[❌ Build Stop]
        
        G --> I[📈 Report Generation]
        H --> I
    end
    
    subgraph "SonarQube Integration"
        J[Jenkins Pipeline] --> A
        K[GitHub Repository] --> C
        L[Quality Dashboard] --> I
    end
    
    style F fill:#e8f5e8
    style G fill:#c8e6c9
    style H fill:#ffcdd2
```

## 🔒 Trivy Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "Trivy Security Scan Process"
        A[📥 Jenkins Trigger<br/>After Docker Push] --> B[🏷️ Image Identification<br/>onurguler18/aws-pipeline:latest]
        B --> C[📚 CVE Database Sync]
        C --> D[🔍 Vulnerability Scan<br/>HIGH, CRITICAL]
        
        subgraph "Scan Types"
            D1[🐳 Container Image Scan]
            D2[📦 Package Vulnerability]
            D3[🔒 Configuration Check]
        end
        
        D --> D1
        D --> D2
        D --> D3
        
        D1 --> E[📊 Security Report]
        D2 --> E
        D3 --> E
        
        E --> F[✅ Scan Complete<br/>Exit Code 0]
        F --> G[📋 Report to Jenkins]
        G --> H[🔄 Continue Pipeline]
    end
    
    subgraph "Trivy Integration"
        I[Jenkins Pipeline] --> A
        J[Docker Hub Registry] --> B
        K[CVE Database] --> C
        L[Jenkins Console] --> G
    end
    
    style A fill:#fff3e0
    style E fill:#ffebee
    style F fill:#c8e6c9
    style H fill:#e8f5e8
```

## 🔄 ArgoCD Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "ArgoCD GitOps Process"
        A[📥 Jenkins Trigger<br/>API Token] --> B[🔄 ArgoCD Application<br/>devops-application]
        B --> C[🔍 GitOps Repository Monitor<br/>aws-pipeline-gitops]
        C --> D[📋 Manifest Analysis<br/>deployment.yaml<br/>service.yaml]
        D --> E[🔄 Desired State Check]
        
        subgraph "Sync Process"
            E1[🔍 Current State Analysis]
            E2[📊 Drift Detection]
            E3[🔄 Sync Decision<br/>Auto Sync]
            E4[⚙️ Kubernetes Apply]
        end
        
        E --> E1
        E1 --> E2
        E2 --> E3
        E3 --> E4
        
        E4 --> F[📦 Pod Creation]
        F --> G[📊 Status Update]
        G --> H[🔔 Notification]
        
        subgraph "Rollback Process"
            I[📋 Rollback Request] --> J[🔄 Previous State]
            J --> K[⚙️ State Restore]
            K --> L[📊 Rollback Complete]
        end
        
        H --> I
    end
    
    subgraph "ArgoCD Integration"
        M[Jenkins API Token] --> A
        N[GitHub GitOps Repo] --> C
        O[Kubernetes EKS Cluster] --> E4
        P[ArgoCD Dashboard] --> G
    end
    
    style A fill:#fff3e0
    style E3 fill:#e8f5e8
    style E4 fill:#c8e6c9
    style L fill:#c8e6c9
```

## 🌐 GitHub Detaylı Süreç Diyagramı

```mermaid
graph TB
    subgraph "GitHub Repository Process"
        A[👨‍💻 Developer Push] --> B[📋 Code Push to Main]
        B --> C[🚀 Webhook Trigger]
        
        subgraph "Webhook Events"
            C1[📥 Push Event]
            C2[🔄 Pull Request Event]
            C3[🏷️ Tag Event]
        end
        
        C --> C1
        C --> C2
        C --> C3
        
        C1 --> D[📤 Jenkins Webhook]
        C2 --> D
        C3 --> D
    end
    
    subgraph "GitHub Integration"
        E[Jenkins Pipeline] --> D
        F[GitHub Repository<br/>aws-pipeline] --> C
        G[GitOps Repository<br/>aws-pipeline-gitops] --> H[ArgoCD Monitor]
    end
    
    style A fill:#e1f5fe
    style C fill:#fff3e0
    style D fill:#fff3e0
    style H fill:#e0f2f1
```

## 📊 Tam Entegrasyon Detay Diyagramı

```mermaid
graph TB
    subgraph "Complete DevOps Integration"
        subgraph "Source Control"
            GH[📁 GitHub<br/>aws-pipeline]
            DEV[👨‍💻 Developer]
        end
        
        subgraph "CI Pipeline"
            J[🚀 Jenkins]
            SQ[🔍 SonarQube]
            D[🐳 Docker]
            T[🔒 Trivy]
        end
        
        subgraph "Container Registry"
            DH[📦 Docker Hub]
        end
        
        subgraph "GitOps & Orchestration"
            GH_GITOPS[📁 GitHub GitOps<br/>aws-pipeline-gitops]
            A[🔄 ArgoCD]
            K[⚙️ Kubernetes EKS]
        end
        
        subgraph "Monitoring"
            P[📊 Prometheus]
            G[📈 Grafana]
        end
    end
    
    DEV -->|Push Code| GH
    GH -->|Webhook| J
    J -->|Test & Build| SQ
    J -->|Quality Check| SQ
    SQ -->|Quality Gate| J
    J -->|Build & Push| D
    D -->|Push Image| DH
    DH -->|Image Ready| T
    T -->|Scan Complete| J
    J -->|Trigger API| A
    GH_GITOPS -->|Manifest Changes| A
    A -->|GitOps Sync| K
    K -->|Pull Image| DH
    K -->|Pod Status| A
    J -->|Metrics| P
    K -->|Metrics| P
    P -->|Data Source| G
    
    style GH fill:#f3e5f5
    style J fill:#fff3e0
    style SQ fill:#e8f5e8
    style T fill:#ffebee
    style D fill:#e3f2fd
    style DH fill:#f1f8e9
    style K fill:#fce4ec
    style A fill:#e0f2f1
    style P fill:#ffebee
    style G fill:#f3e5f5
    style GH_GITOPS fill:#f3e5f5
```

## 🔄 Pipeline Fail Scenarios Diyagramı

```mermaid
graph TB
    subgraph "Pipeline Failure Scenarios"
        A[🚀 Pipeline Start] --> B[🧪 Test Maven]
        B --> C{Test Results}
        C -->|Pass| D[🔨 Build Maven]
        C -->|Fail| E1[❌ Test Failure]
        
        D --> E[🔍 SonarQube Analysis]
        E --> F{Quality Gate}
        F -->|Pass| G[🐳 Docker Build & Push]
        F -->|Fail| E2[❌ Quality Failure]
        
        G --> H[📦 DockerHub Registry]
        H --> I[🔒 Trivy Scan]
        I --> J[✅ Continue Pipeline]
        J --> K[🔄 Trigger ArgoCD]
        K --> L[✅ Success]
    end
    
    subgraph "Failure Handling"
        E1 --> N1[❌ Pipeline Stop]
        E2 --> N2[❌ Pipeline Stop]
        
        N1 --> O[🔧 Developer Fix]
        N2 --> O
        
        O --> P[🔄 Retry Pipeline]
        P --> A
    end
    
    style C fill:#ffebee
    style F fill:#ffebee
    style E1 fill:#ffcdd2
    style E2 fill:#ffcdd2
    style L fill:#c8e6c9
```
