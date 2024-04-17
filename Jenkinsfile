pipeline {
    agent any
    environment {
         SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
         stage('sonar') {
            steps {
              withSonarQubeEnv('sonar-server') {
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=paymentservice \
                    -Dsonar.projectKey=paymentservice '''
                   }
            }
        }
         stage('docker-build') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t paymentservice:latest ."
                       sh  "docker tag paymentservice:latest meena835/paymentservice:latest"
                  }
               }
            }
        }
        stage('trivy') {
            steps {
              sh  "trivy image --format json -o report.json meena835/paymentservice:latest"
            }
        }
         stage('docker-push') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                       sh  "docker push  meena835/paymentservice:latest"
                  }
               }
            }
        }
    }
}
