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
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=6071363d8c25c3ae8fd1ed69ba707251247dac01 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**/TaskBackendApplication.java,**/RootController.java"
                }
            }
        }
        stage('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'tomcat_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
         stage('API test') {
            steps {
                dir('api-test') {
                    git branch: 'main', url: 'https://github.com/rcarauta/tasks-api-test.git'
                    sh 'mvn clean test'
                }
            }
        }
        stage('Deploy Frontend') {
            steps {
                dir('frontend') {
                    git branch: 'master', url: 'https://github.com/rcarauta/tasks-frontend.git'
                    sh 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'tomcat_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
                
            }
        }
    }
}

