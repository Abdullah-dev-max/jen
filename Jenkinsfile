


pipeline {
    agent any
    

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SONAR_ORGANIZATION = 'jenkins-project-123'
        SONAR_PROJECT_KEY = 'jenkins-project-123_ci-jenkins'
    }

    stages {
        
        stage('Code-Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
  -Dsonar.organization=jenkins-project-123 \
  -Dsonar.projectKey=jenkins-project-123_ci-jenkins \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io '''
                }
            }
        }
       
        
      
       stage('Docker Build And Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-cred') {
                        def buildNumber = env.BUILD_NUMBER ?: '1'
                        def image = docker.build("ahmad762249/jen:latest")
                        image.push()
                    }
                }
            }
        }
    
       
        stage('Deploy To EC2') {
            steps {
                script {
                        sh 'docker rm -f $(docker ps -q) || true'
                        sh 'docker run -d -p 3000:3000 ahmad762249/jen:latest'
                        
                    
                }
            }
        }
        
}
}
