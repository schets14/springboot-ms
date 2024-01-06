pipeline {
    agent {
        docker {
            image 'maven:latest' // Specify the Maven version you need
            args '-u root' // Mount the Maven cache for caching dependencies
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
                sh 'pwd'
                sh 'mvn clean install'
            }
        }
        stages {
            stage('BUILD and PUSH') {
                environments {
                    DOCKER_IMAGE = "schets14/myimages:${BUILD_NUMBER}"
                    DOCKERHUB_CRED = credentials('docker-cred')
                }
                steps {
                    sh 'pwd's
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CRED) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
