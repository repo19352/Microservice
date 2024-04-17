pipeline {
    agent any
    environment {
         SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
         stage('sonar') {
            steps {
              withSonarQubeEnv('sonar-server') {
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=adservice \
                    -Dsonar.projectKey=adservice '''
                   }
            }
        }
         stage('docker-build') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t adservice:latest ."
                       sh  "docker tag adservice:latest meena835/adservice:latest"
                  }
               }
            }
        }
        stage('trivy') {
            steps {
              sh  "trivy image --format json -o report.json meena835/adservice:latest"
            }
        }
         stage('docker-push') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                       sh  "docker push  meena835/adservice:latest"
                  }
               }
            }
        }
    }
}
