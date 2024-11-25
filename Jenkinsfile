


pipeline {
    agent any
    

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('sonar-token')
        SONAR_ORGANIZATION = 'ehino'
        SONAR_PROJECT_KEY = 'ehisakhile_jenkins-sonarcube-project'
    }

    stages {
        
        stage('Code-Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.organization=ehino \
                    -Dsonar.projectKey=ehisakhile_jenkins-sonarcube-project \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://sonarcloud.io
                    '''
                }
            }
        }
       
        
      
       stage('Docker Build And Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-login') {
                        def buildNumber = env.BUILD_NUMBER ?: '1'
                        def image = docker.build("ehisakhile/jenkins-sonarcube-project:1.0.0")
                        image.push()
                    }
                }
            }
        }
    
       
        stage('Deploy To EC2') {
            steps {
                script {
                        sh 'docker rm -f $(docker ps -q) || true'
                        sh 'docker run -d -p 3000:3000 ehisakhile/jenkins-sonarcube-project'
                        
                    
                }
            }
        }
        
}
}
