pipeline {
    agent any
        stages {
            stage('Build Backend'){
                steps {
                    bat 'mvn clean package -DskipTests=true'
                }
            }
            stage('Unit Test'){
                steps {
                    bat 'mvn test'
                }
            }
            stage('Sonar Analisys'){
                environment {
                    scannerHome = tool 'SONAR_REMOTO_SCANNER'
                }
                steps {
                    withSonarQubeEnv('SONAR_SERVIDOR'){
                        bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.210.20:9000 -Dsonar.login=707d09e67584349bebb47538eba278d844126962 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                    }
                }
            }

            stage('Quality Gate') {
                steps {
                    waitForQualityGate abortPipeline: false
                }
            } 

            stage('Deploy Backend') {
                steps {
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
                }
            }
        }

        post {
        
            success {
                echo 'This will run only if successful'
            }
            failure {
                echo 'This will run only if failed'
            }
    
        }
}
