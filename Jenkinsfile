pipeline{
    agent any
    tools{
        maven 'local_maven'
    }
    stages{
        stage ('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                // script{
                //     readProp = readProperties file: 'build.properties'
                // }
                // echo "This is running on ${readProp['deploy.type']}"
                deploy adapters: [tomcat9(credentialsId: '2f95a0f8-17f9-4674-a61b-474d7bd55c75', path: '', url: 'http://127.0.0.1:8090/')], contextPath: null, war: "**/*.war"
            }
        }
    }
}