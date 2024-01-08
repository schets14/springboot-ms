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
        PREVIOUS_BUILD_NUMBER = script {
                    def previousBuild = currentBuild.getPreviousBuild()
                    return previousBuild ? previousBuild.getNumber().toString() : 'N/A'
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
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_T')]) {
                    sh '''
                    pwd
                    echo "${previousBuildNumber}"
                    git config user.email "schets14@gmail.com"
                    git config user.name "Chetan Solanki"
                    BUILD_NUMBER=$BUILD_NUMBER
                    echo $PREVIOUS_BUILD_NUMBER
                    sed -i "s/$PREVIOUS_BUILD_NUMBER/$BUILD_NUMBER/g" deployment.yaml
                    git add deployment.yaml
                    git commit -m "Update deployment image to version $BUILD_NUMBER"
                    git push https://$GITHUB_T@github.com/$GIT_USER_NAME/$GIT_REPO_NAME HEAD:main
                '''
                }
            }

        }

    }
}
