pipeline {
    agent {
        docker {
            image 'abhishekf5/maven-abhishek-docker-agent:v1' // Specify the Maven version you need
            args '-u root' // Mount the Maven cache for caching dependencies
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        DOCKERHUB_CRED = 'docker-cred'
        registry = 'schets14/myimages'
        dockerImage = ''
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
        stage('Building image') {
            steps{
                script {
                    echo 'building image'
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Pushing image') {
            steps{
                script {
                    echo 'pushing image on repo'
                    docker.withRegistry('', DOCKERHUB_CRED) {
                        dockerImage.push()
                    }
                }
            }
        }

    }
}
