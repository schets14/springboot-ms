pipeline {
    agent {
        docker {
            image 'abhishekf5/maven-abhishek-docker-agent:v1' // Specify the Maven version you need
            args '-u root' // Mount the Maven cache for caching dependencies
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        DOCKERHUB_CRED = 'dockerhub'
        registry = 'schets14/myimages'
        dockerImage = ''
        GIT_REPO_NAME = 'springboot-ms'
        GIT_USER_NAME = 'schets14'
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
        stage('Updating Deployment file'){
            steps{
                sh '''
                    pwd
                    git config user.email "schets14@gmail.com"
                    git config user.name "Chetan Solanki"
                    BUILD_NUMBER=$BUILD_NUMBER
                    sed -i "s/replaceImageTag/$BUILD_NUMBER/g" deployment.yaml
                    git add deployment.yaml
                    git commit -m "Update deployment image to version $BUILD_NUMBER"
                    git push https://$GITHUB_TOKEN@github.com/$GIT_USER_NAME/$GIT_REPO_NAME HEAD:main
                '''
            }

        }

    }
}
