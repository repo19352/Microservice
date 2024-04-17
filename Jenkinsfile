pipeline {
    agent any
    environment {
         SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
        stage('git-checkout') {
            steps {
                git branch: 'shippingservice', url: 'https://github.com/repo19352/Microservice.git'
            }
        }
         stage('sonar') {
            steps {
              withSonarQubeEnv('sonar-server') {
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=shippingservice \
                    -Dsonar.projectKey=shippingservice '''
                   }
            }
        }
         stage('docker-build') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t shippingservice:latest ."
                       sh  "docker tag shippingservice:latest meena835/shippingservice:latest"
                  }
               }
            }
        }
        stage('trivy') {
            steps {
              sh  "trivy image --format json -o report.json meena835/shippingservice:latest"
            }
        }
         stage('docker-push') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                       sh  "docker push  meena835/shippingservice:latest"
                  }
               }
            }
        }
    }
}

