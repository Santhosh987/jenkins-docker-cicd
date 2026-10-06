pipeline {

    agent any

    stages {
        stage('Checkout'){
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker run --rm \
                    -v "$WORKSPACE/app:/app" \
                    -w /app \
                    --entrypoint sh \
                    node:22-alpine \
                    -c "npm install && npm test"
                    ''' 
            }
        }

        stage("Docker Build") {
            steps {
                sh '''
                    docker build -t jenkins-docker-cicd:${BUILD_NUMBER} .
                    '''
            }
        }

        stage("ECR Login") {
            steps {
                sh '''
                    aws ecr get-login-password --region ap-south-1 | \
                    docker login --username AWS --password-stdin \
                    "937792903959.dkr.ecr.ap-south-1.amazonaws.com"
                    '''
            }
        }

        stage("Image Tagging") {
            steps {
                sh '''
                    docker tag jenkins-docker-cicd:${BUILD_NUMBER} \
                    937792903959.dkr.ecr.ap-south-1.amazonaws.com/jenkins-docker-cicd:${BUILD_NUMBER}
                    '''
            }
        }

        stage("Image Push") {
            steps {
                sh '''
                    docker push 937792903959.dkr.ecr.ap-south-1.amazonaws.com/jenkins-docker-cicd:${BUILD_NUMBER}
                    '''
            }
        }
    }
}