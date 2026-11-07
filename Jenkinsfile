pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t myapp:$BUILD_NUMBER .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm -v $WORKSPACE:/app -w /app myapp:$BUILD_NUMBER pytest --junitxml=results.xml'
            }
            post {
                always {
                    junit 'results.xml'
                }
            }
        }
        stage('Publish') {
            steps {
                archiveArtifacts artifacts: 'results.xml', fingerprint: true
            }
        }
    }
}