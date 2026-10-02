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
    }
}