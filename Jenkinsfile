pipeline {
    agent {
        docker {
            image 'maven:latest' // Specify the Maven version you need
        }
    }
    stages {
        stage('Code-Checkout') {
            steps {
                echo 'Checking out code from git repo'
                git url: 'https://github.com/schets14/springboot-ms.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo 'building'
                sh 'mvn clean install'
            }
        }
    }
}
