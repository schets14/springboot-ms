pipeline {
    agent {
        docker {
            image 'maven:latest' // Specify the Maven version you need
            args '-v $HOME/.m2:/root/.m2' // Mount the Maven cache for caching dependencies
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
