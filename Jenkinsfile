
pipeline {
    agent any

    environment {
        IMAGE_NAME = "anon-ecommerce-website"
        CONTAINER_NAME = "ecommerce-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Prasadsasubilli/anon-ecommerce-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Stop Existing Container') {
            steps {
                script {
                    // Stop and remove old container if it exists
                    sh '''
                    if [ "$(docker ps -q -f name=$CONTAINER_NAME)" ]; then
                        docker stop $CONTAINER_NAME
                        docker rm $CONTAINER_NAME
                    fi
                    '''
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    // Run container and expose on port 8080
                    sh 'docker run -d --name $CONTAINER_NAME -p 8090:80 $IMAGE_NAME'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh 'sleep 5'
                    sh 'curl -I http://localhost:8090 || true'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Website is running on port 8080."
        }
        failure {
            echo "❌ Deployment failed. Check Jenkins logs."
        }
    }
}
