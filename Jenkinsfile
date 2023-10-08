pipeline {
    agent {
        docker {
            image 'node:7.8.0'
        }
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                dir('src') {
                    sh 'npm install'
                    // Add your test commands here
                }
            }
        }

        stage('Build Docker Image') {
            when {
                expression { env.BRANCH_NAME == 'dev' }
            }
            steps {
                dir('src') {
                    sh 'npm run build'
                    // Add commands to copy 'public' and 'src' directories to your Docker image
                }
                // Build and tag Docker image
                // script {
                //     docker.build('your-image-name:main')
                // }
            }
        }

        // stage('Deploy') {
        //     when {
        //         expression { env.BRANCH_NAME == 'main' }
        //     }
        //     steps {
        //         // Deploy your application using the Docker image
        //         script {
        //             docker.withRegistry('https://your-docker-registry', 'your-docker-credentials') {
        //                 docker.image('your-image-name:main').push()
        //                 // Deploy your application with the appropriate port (e.g., 3000 for main)
        //             }
        //         }
        //     }
        // }
    }
}
