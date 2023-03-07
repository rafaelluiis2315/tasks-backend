pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage ('API Test') {
            steps {
                dir('api-test') {
                    git credentialsId: '14cda81d-84da-4317-95ae-8b65117a2296', url: 'https://github.com/rafaelluiis2315/tasks-api-test.git'
                    bat 'mvn test'
                }
            }
        }
        stage ('Deploy Frontend') {
            steps {
                dir('frontend') {
                    git credentialsId: '14cda81d-84da-4317-95ae-8b65117a2296', url: 'https://github.com/rafaelluiis2315/tasks-frontend.git'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage ('Functional Test') {
            steps {
                dir('functional-test') {
                    git credentialsId: '14cda81d-84da-4317-95ae-8b65117a2296', url: 'https://github.com/rafaelluiis2315/petros-automation.git'
                    bat './gradlew runPoshi'
                }
            }
        }
        
    }
}


