pipeline {
    agent any

    stages {

        stage('Build'){
            steps {

                script{
                    def nodejsTool = tool name: 'node-20-tool', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodejsTool}/bin:${env.PATH}"
                }
                sh '''
                echo "Building the application..."
                npm install
                npm run-script build
                '''
            }
        }

        stage('Docker'){
            steps {
                script{
                    def dockerTool = tool name: 'docker-latest-tool', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
                    env.PATH = "${dockerTool}/bin:${env.PATH}"
                }
                sh '''
                    echo "Dockerizing the application..."
                    docker --version
                    docker images

                    docker build -t ryanaugustyn/react-jenkins-docker:1 .

                    docker images
                '''

                //Access the personal-docker-credentials
                //Use the to login to Docker through the login CLI command
                
                // withCredentials([usernamePassword(credentialsId: 'personal-dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASWORD')]){
                // sh "echo ${DOCKER_USERNAME}"
                // //Push the image to your personal Docker Hub repo
                // sh 'docker push ryanaugustyn/react-jenkins-docker:1'
                // }
            }
        }
    }
}