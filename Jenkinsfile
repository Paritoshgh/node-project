pipeline {
    agent { label 'agent1' }
    stages {
        stage('Code') {
            steps {
                echo 'Cloning from GIT'
                git branch: 'master', url: 'https://github.com/Paritoshgh/node-project.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
                sh 'docker build . -t ghadgeparitosh10/node-todo:latest'
            }
        }
        stage('Push') {
            steps {
                echo 'Push Image to Docker Hub'
                withCredentials([
                usernamePassword(
                credentialsId: 'DockerHub',
                passwordVariable: 'DockerHubPassword',
                usernameVariable: 'DockerHubUser'
                )]) {
                    sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPassword}"
                    sh 'docker push ghadgeparitosh10/node-todo:latest'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'docker run --rm ghadgeparitosh10/node-todo:latest sh -c "npx mocha test.js"'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to PROD'
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
