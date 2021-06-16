pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool "SONAR_SCANNER"
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=6071363d8c25c3ae8fd1ed69ba707251247dac01-Dsonar.java.binaries=target-Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**/TaskBackendApplication.java,**/RootController.java"
                }
            }
        }
    }
}

