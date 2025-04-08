pipeline {
    agent any
    tools {
        maven 'Maven3' // Ensure "Maven3" is the label you configured in Jenkins Global Tools
    }

    stages {
        stage('Clean and Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }
        stage('Package') {
            steps {
                bat 'mvn package'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat "docker build -t %IMAGE_NAME%:%BUILD_NUMBER% ."
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Dockerid', passwordVariable: 'Dockerpwd', usernameVariable: 'Dockerid')]) {
                    bat "docker login -u ramya739 -p %Dockerpwd%"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                bat "docker push %IMAGE_NAME%:%BUILD_NUMBER%"
            }
        }
        stage('Run Docker Container') {
            steps {
                // Optional: Stop existing container before running new one
                bat "docker run -itd -p 8080:8081 %IMAGE_NAME%:%BUILD_NUMBER%"
            }
        }
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

