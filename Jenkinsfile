pipeline {
    agent {
        docker {
            image 'node:7.8.0'
            args '-p 3000:3000'
        }
    }
    
    stage('Build') {
        steps {
            dir('src') {
                sh 'npm install'
            }
        }
    }
}