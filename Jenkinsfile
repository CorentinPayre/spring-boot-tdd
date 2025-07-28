pipeline {
    agent any
    stages {
        stage('Télécharger') {
            steps {
                git 'https://github.com/CorentinPayre/spring-boot-tdd.git'
            }
        }
        stage('Compiler') {
            steps {
                bat "echo 'compilation...'"
                bat "mvn clean compile -DskipTests"
            }
        }
        stage('Tests') {
            steps {
                bat "echo 'test...'"
                bat "mvn test"
                
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
        }
        }
                stage('Packaging') {
            steps {
                bat "mvn package -DskipTests"
            }
        }
                        stage('Create image') {
            steps {
                bat "docker build -t wisdom-pet ."
            }
        }
                                        stage('Stop and remove container') {
            steps {
                bat "docker stop demo-isika || exit /b 0"
            }
        }
                                stage('Run container') {
            steps {
                bat "docker run -d --rm -p 8180:8180 --name demo-isika  wisdom-pet "
            }
        }
    }
}
