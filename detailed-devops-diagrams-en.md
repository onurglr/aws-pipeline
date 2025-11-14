# Detailed DevOps Tool Diagrams

> **🇹🇷 Turkish Version:** [detailed-devops-diagrams.md](detailed-devops-diagrams.md)

## 🚀 Jenkins Detailed Process Diagram

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
    
    subgraph "External System Connections"
        P[GitHub Repository<br/>Code Source] -->|Sends Webhook| A
        Q[SonarQube Server<br/>Code Quality] -->|Performs Analysis| E
        H -->|Image Push| R[Docker Hub Registry<br/>Image Storage]
        S[Trivy Scanner<br/>Security Scanning] -->|Performs Scan| J
        M -->|API Request| T[ArgoCD API<br/>CD Trigger]
    end
    
    style A fill:#fff3e0
    style F fill:#e8f5e8
    style K fill:#ffebee
    style I fill:#ffcdd2
    style N fill:#c8e6c9
```

## 🐳 Docker Detailed Process Diagram

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
    
    subgraph "External System Connections"
        H[Jenkins Pipeline<br/>CI/CD Orchestrator] -->|Triggers| A
        E -->|Image Push| I[Docker Hub Registry<br/>Container Storage]
        J[Trivy Scanner<br/>Security Scanning] -->|Performs Scan| G
        F -->|Image Pull| K[Kubernetes Cluster<br/>Image Pull]
    end
    
    style A fill:#fff3e0
    style E fill:#e8f5e8
    style G fill:#ffebee
    style F fill:#f1f8e9
```

## ⚙️ Kubernetes Detailed Process Diagram

```mermaid
graph TB
    subgraph "Kubernetes Deployment"
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
    
    subgraph "External System Connections"
        L[ArgoCD GitOps<br/>Deployment Management] -->|Triggers Sync| A
        C -->|Image Pull| M[Docker Hub<br/>Image Source]
        H -->|Sends Metrics| N[Prometheus<br/>Metric Collection]
        L -->|Monitors Manifest| O[GitHub GitOps Repo<br/>Manifest Source]
    end
    
    style A fill:#e0f2f1
    style E2 fill:#e8f5e8
    style E3 fill:#e8f5e8
    style E4 fill:#c8e6c9
```

## 🔍 SonarQube Detailed Process Diagram

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
    
    subgraph "External System Connections"
        J[Jenkins Pipeline<br/>CI/CD Trigger] -->|Triggers| A
        K[GitHub Repository<br/>Code Source] -->|Pulls Code| C
        I -->|Shows Report| L[Quality Dashboard<br/>Report Viewing]
    end
    
    style F fill:#e8f5e8
    style G fill:#c8e6c9
    style H fill:#ffcdd2
```

## 🔒 Trivy Detailed Process Diagram

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
    
    subgraph "External System Connections"
        I[Jenkins Pipeline<br/>Scan Trigger] -->|Triggers| A
        B -->|Reads Image| J[Docker Hub Registry<br/>Image Source]
        C -->|Fetches CVE Data| K[CVE Database<br/>Security Database]
        G -->|Sends Report| L[Jenkins Console<br/>Report Viewing]
    end
    
    style A fill:#fff3e0
    style E fill:#ffebee
    style F fill:#c8e6c9
    style H fill:#e8f5e8
```

## 🔄 ArgoCD Detailed Process Diagram

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
    
    subgraph "External System Connections"
        M[Jenkins API Token<br/>CD Pipeline Trigger] -->|Triggers| A
        C -->|Monitors Manifest| N[GitHub GitOps Repo<br/>aws-pipeline-gitops]
        E4 -->|Applies| O[Kubernetes EKS Cluster<br/>Deployment Target]
        G -->|Shows Status| P[ArgoCD Dashboard<br/>Status Viewing]
    end
    
    style A fill:#fff3e0
    style E3 fill:#e8f5e8
    style E4 fill:#c8e6c9
    style L fill:#c8e6c9
```

## 🌐 GitHub Detailed Process Diagram

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
    
    subgraph "External System Connections"
        D -->|Receives Webhook| E[Jenkins Pipeline<br/>Webhook Receiver]
        F[GitHub Repository<br/>aws-pipeline<br/>Main Code Repo] -->|Push Event| C
        H[ArgoCD Monitor<br/>Monitoring] -->|Monitors Manifest| G[GitOps Repository<br/>aws-pipeline-gitops<br/>Manifest Repo]
    end
    
    style A fill:#e1f5fe
    style C fill:#fff3e0
    style D fill:#fff3e0
    style H fill:#e0f2f1
```

## 📊 Complete Integration Detail Diagram

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
    J -->|Trigger Scan| T
    T -->|Read Image| DH
    T -->|Scan Complete| J
    J -->|Trigger API| A
    A -->|Monitor Repo| GH_GITOPS
    GH_GITOPS -->|Manifest Changes| A
    A -->|GitOps Sync| K
    K -->|Pull Image| DH
    K -->|Pod Status| A
    J -->|Send Metrics| P
    K -->|Send Metrics| P
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

## 🔄 Pipeline Fail Scenarios Diagram

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

