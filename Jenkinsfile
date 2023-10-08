pipeline {
    agent {
        docker {
            image 'node:7.8.0'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                dir('src') {
                    sh 'npm install'
                }
            }
        }
    }
}
