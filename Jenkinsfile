pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImageTag = 'nodedev:v1.0'

                    if (env.BRANCH_NAME == 'main') {
                        dockerImageTag = 'nodemain:v1.0'
                    }

                    sh "docker stop my-app || true" // Stop previously running containers (if any)
                    sh "docker rm my-app || true"   // Remove previously running containers (if any)
                    sh "docker build -t ${dockerImageTag} ."
                }
            }
        }

        stage('Run Docker Container') {
            when {
                anyOf {
                    branch 'main'
                    branch 'dev'
                }
            }
            steps {
                script {
                    def dockerImageTag = 'nodedev:v1.0'
                    def port = 3000 // Default port for 'dev' branch

                    if (env.BRANCH_NAME == 'main') {
                        dockerImageTag = 'nodemain:v1.0'
                        port = 3001 // Port for 'main' branch
                    }

                    sh "docker run -d --expose ${port} -p ${port}:${port} --name my-app ${dockerImageTag}"
                }
            }
        }
    }

    post {
        always {
            // Cleanup (stop and remove) containers if the pipeline is aborted or finished
            sh "docker stop my-app || true"
            sh "docker rm my-app || true"
        }
    }
}
