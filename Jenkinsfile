pipeline{
    agent any
    tools{
        maven 'local_maven'
    }
    stages{
        stage ('Build'){
            steps{
                bat 'mvn clean package'
            }
            post{
                success{
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv() {
                  sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=Jenkins-sonar -Dsonar.projectName='Jenkins-sonar'"
                }
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://127.0.0.1:8100/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
