pipeline {
    agent any
    environment {
        IMAGE = 'eliot-jenkins-flask'
        TAG   = 'latest'
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/EliotMutoriti/Eliot-DevOps.git'
            }
        }
        stage('Build & Deploy') {
            steps {
                sh '''
                    docker compose down --remove-orphans || true
                    docker compose up -d --build
                    sleep 5
                    curl -f http://localhost:5000 || (docker compose logs flask && exit 1)
                '''
            }
        }
    }
    post {
        success { echo "App is LIVE at http://$(curl -s ifconfig.me):5000" }
        failure { sh 'docker compose logs' }
    }
}
