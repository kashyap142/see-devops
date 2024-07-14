pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS 20.14.0'
        PATH = "${env.NODEJS_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                git branch: 'master', 
                url: 'https://github.com/kashyap142/see-devops.git'
                echo 'Checkout completed.'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
                echo 'Dependencies installed.'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                bat 'npm run build'
                echo 'Build completed.'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'npm test'
                echo 'Tests completed.'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    bat 'docker build -t kkashyap142/see-devops:latest .'
                }
                echo 'Docker image built.'
            }
        }

        // stage('Login to Docker Hub') {
        //     steps {
        //         echo 'Logging in to Docker Hub...'
        //         script {
        //             withCredentials([string(credentialsId: 'dockerhub-username', variable: 'DOCKER_USERNAME'), 
        //                              string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
        //                 bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
        //             }
        //         }
        //         echo 'Logged in to Docker Hub.'
        //     }
        // }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image...'
                bat 'docker push kkashyap142/see-devops:latest'
                echo 'Docker image pushed.'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                bat 'docker run -d -p 3000:3000 kkashyap142/see-devops:latest'
                echo 'Docker container running.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
            bat 'docker ps -a'
            bat 'docker images'
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
