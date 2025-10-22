pipeline {
    agent {
        node {
            label 'My-Jenkins-Agent'
        }
    }
    //agent any
    tools {
        maven 'Maven3'
        jdk 'Java21'
    }

    environment {
        APP_NAME = "devops-03-pipeline-aws"
        RELEASE = "1.0"
        DOCKER_USER = "onurguler18"
        DOCKER_LOGIN = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}.${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }

    stages {
        stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/onurglr/aws-pipeline']])
            }
        }

        stage('Test Maven') {
            steps {
                script {
                    if (isUnix()) {
                        // Linux or MacOS
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'  // Windows
                    }
                }
            }
        }
    

        stage('Build Maven') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }



        stage("SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonar-token') {
                        if (isUnix()) {
                            // Linux or MacOS
                            sh "mvn sonar:sonar"
                        } else {
                            bat 'mvn sonar:sonar'  // Windows
                        }
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonar-token'
                }
            }
        }
  


        stage('Build & Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_LOGIN) {
                        docker_image = docker.build "${IMAGE_NAME}"
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }



        stage("Trivy Scan") {
            steps {
                script {
                    if (isUnix()) {
                        sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image mimaraslan/devops-03-pipeline-aws:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                    } else {
                        bat ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image mimaraslan/devops-03-pipeline-aws:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                    }
                }
            }
        }



        stage('Cleanup Old Docker Images') {
            steps {
                script {
                    if (isUnix()) {
                        // Bu repo için tüm image’leri al, tarihe göre sırala, son 3 hariç sil
                        sh """
                            docker images "${env.IMAGE_NAME}" --format "{{.Repository}}:{{.Tag}} {{.CreatedAt}}" \\
                            | sort -r -k2 \\
                            | tail -n +4 \\
                            | awk '{print \$1}' \\
                            | xargs -r docker rmi -f
                        """

                    } else {
                        bat """
                             for /f "skip=3 tokens=1" %%i in ('docker images ${env.IMAGE_NAME} --format "{{.Repository}}:{{.Tag}}" ^| sort') do docker rmi -f %%i
                        """
                    }
                }
            }
        }





         stage('Deploy Kubernetes') {
             steps {
             script {
                     kubernetesDeploy (configs: 'deployment-service.yaml', kubeconfigId: 'kubernetes')
                 }
             }
         }


         stage('Docker Image to Clean') {
             steps {

                    //  sh 'docker image prune -f'
                      bat 'docker image prune -f'

             }
         }


        stage("Trigger CD Pipeline") {
            steps {
                script {
                    sh "curl -v -k --user admin:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-35-169-94-227.compute-1.amazonaws.com:8080/job/devops-03-pipeline-aws-gitops-v2/buildWithParameters?token=GITOPS_TRIGGER_START'"
                }
            }
        }






    }
}