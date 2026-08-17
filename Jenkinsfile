
pipeline {
    agent any

    tools {
    jdk 'JDK'
    maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Java application...'
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                bat 'mvn package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying application to Tomcat...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'tomcat-credentials',
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASS'
                    )
                ]) {
                    bat '''
                    for %%f in (target\\*.war) do curl --upload-file "%%f" -u %TOMCAT_USER%:%TOMCAT_PASS% "http://localhost:8081/manager/text/deploy?path=/jenkins-java-app&update=true"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}
       
           
