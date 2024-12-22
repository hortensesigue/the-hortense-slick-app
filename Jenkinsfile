pipeline {
    agent any
    environment {
        DOCKER_REPOSITORY = 'hortensehouendji/my-hortense-slick-app'
        DOCKERHUB_CREDENTIALS_ID = credentials('DockerHubcreds')
    }
   
    stages {
        stage('Source') {
            steps {
                echo 'retrieving the code from github'
                git branch: 'main', credentialsId: 'GithubCreds', url: 'https://github.com/hortensesigue/the-hortense-slick-app.git'
                
                sh 'ls -l'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building my slick app container image'
                sh 'docker build -t $DOCKER_REPOSITORY:v$BUILD_NUMBER .'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running my slick app container'
                sh 'docker run --name my-hotense-slick-app-v$BUILD_NUMBER -dit -p 80:80 $DOCKER_REPOSITORY:v$BUILD_NUMBER'
            }
        }
        
        stage('Manual Approval') {
            steps {
                echo 'This is for Manual approval'
                input 'Approve Deployment to production'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Lets log into our DockerHb'
                sh 'docker login -u$DOCKERHUB_CREDENTIALS_ID_USR -p$DOCKERHUB_CREDENTIALS_ID_PSW'
                
                echo 'Lets push our Docker Image to DockerHb'
                sh 'docker push $DOCKER_REPOSITORY:v$BUILD_NUMBER'
                
                sh 'echo the following docker image of my slick app was pushed successfully to DockerHub $DOCKER_REPOSITORY:v$BUILD_NUMBER'
            }
        }
    }
    
    post { 
        always { 
            echo 'Stoping and Deleting the container '
            sh 'docker stop my-hotense-slick-app-v$BUILD_NUMBER'
            sh 'docker rm my-hotense-slick-app-v$BUILD_NUMBER'
        }
    }
}
