pipeline {

    agent any

    stages {
        stage('Checkout'){
            steps {
                checkout scm
            }
        }

        stage('test') {
            steps {
                sh '''
                    docker run --rm \
                    -v "$WORKSPACE/app:/app" \
                    -w /app \
                    node:22-alpine \
                    -c "npm install && npm test"
                    ''' 
            }
        }
    }
}