pipeline {
    agent {
        docker {
            image 'maven:latest' // Specify the Maven version you need
            args '-u root' // Mount the Maven cache for caching dependencies
        }
    }
    environment {
        DOCKERHUB_CRED = credentials('docker-cred')
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
                sh 'pwd'
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('schets14/myimages:spring-ms.v1', '.')
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'pwd'
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CRED) {
                            def customImage = docker.image('schets14/myimages:spring-ms.v1')
                            customImage.push()
                    }
                }
            }
        }
    }
}