pipeline {
    agent { label 'worker' }

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials-id')
        IMAGE_NAME = "svitlanabs2334/forstep2:latest"
    }

    stages {
        stage('Pull Code') {
            steps {
                git 'https://github.com/yourusername/forStep2.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    def testResult = sh(script: 'docker run --rm $IMAGE_NAME node test.js', returnStatus: true)
                    if (testResult != 0) {
                        error("Tests failed")
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh "echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin"
                    sh "docker push $IMAGE_NAME"
                }
            }
        }
    }

    post {
        failure {
            echo 'Tests failed'
        }
    }
}
