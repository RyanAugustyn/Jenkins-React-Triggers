pipeline {
    agent any

    environment{
        def nodejsTool = tool name: 'node-20-tool', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        def dockerTool = tool name: 'docker-latest-tool', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        PATH = "${nodejsTool}/bin:${dockerTool}/bin:${env.PATH}"
    }

    stages {

        stage('Install Dependencies'){
            steps {

                sh 'npm install'
            }
        }

        stage('Create Production React Build'){
            steps {

                sh 'npm run-script build'
            }
        }

        stage('Build Docker Image'){
            steps {
  
                sh '''
                    docker build -t ryanaugustyn/react-jenkins-docker:1 .
                    docker images
                '''
                
                withCredentials([usernamePassword(credentialsId: 'personal-dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASWORD')]){
                sh "echo ${DOCKER_USERNAME}"
                sh 'docker push ryanaugustyn/react-jenkins-docker:1'
                }
            }
        }

        stage('Push Docker Image'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'personal-dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASWORD')]){
                sh "echo ${DOCKER_USERNAME}"
                sh 'docker push ryanaugustyn/react-jenkins-docker:1'
                }
            }
        }

        stage('Deploy New Image to AWS EC2'){
            steps{
                sh 'ech "Deploying to EC2...'
                //SSH into our remote server
                //Shut down the current running image
                //Pull the new image that was just pushed
                //Launch thtat new image running on our remote server
            }
        }
    }
}