pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }
    parameters {
         string(name: 'staging_server', defaultValue: '13.232.37.20', description: 'Remote Staging Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        deploy adapters: [tomcat9(credentialsId: '731972d0-899a-45c6-9906-382e77391c45', path: '', url: 'http://192.168.1.247:8080/')], contextPath: null, war: '**/*.war'
                    }
                }
            }
        }
    }
}
