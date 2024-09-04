Pipeline {
    agent any
    stage('Build Backend'){
        steps {
            bat 'mvn clean package -DskipTests=true'
        }
    }
}
