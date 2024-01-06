pipeline {
    agent {
        docker {
            image 'abhishekf5/maven-abhishek-docker-agent:v1' // Specify the Maven version you need
            args '-u root' // Mount the Maven cache for caching dependencies
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
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
                    def customImage = docker.withRegistry( 'https://index.docker.io/v1/', DOCKERHUB_CRED ) {
                        customImage.push()
                    }
                }
            }
        }
    }
}
