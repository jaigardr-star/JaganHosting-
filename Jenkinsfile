pipeline {
    agent any

    stages {

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jagan-hosting:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f jagan-hosting || true'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker run -d \
                    --name jagan-hosting \
                    -p 8091:8080 \
                    jagan-hosting:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    sleep 5
                    docker ps --filter name=jagan-hosting
                    curl -f http://localhost:8091/
                '''
            }
        }
    }
}
