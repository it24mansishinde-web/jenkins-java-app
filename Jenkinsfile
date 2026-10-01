pipeline {
    agent any

   tools {
    jdk 'JDK'
    maven 'Maven'
}

    stages {

        stage('Get Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Project') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }
    }

    post {
        success {
            echo 'Jenkins Pipeline and SonarQube analysis completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
       
           
