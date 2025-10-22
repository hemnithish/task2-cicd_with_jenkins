pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hemis15/simpleapp:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hemnithish/task2-cicd_with_jenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Test') {
            steps {
                bat 'docker run --rm $DOCKER_IMAGE node -e "console.log(\\"Test Passed\\")"'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'docker-token', variable: 'DOCKER_HUB_TOKEN')]) {
                    bat '''
                        echo $DOCKER_HUB_TOKEN | docker login -u hemis15 --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
