pipeline {
    agent any
    stages{
        stage("code"){
            steps {
                echo "Cloning the code"
                git branch: 'main', url: 'https://github.com/spelluri/docker-flask-app.git'
            }
        }
        stage("build") {
            steps {
                echo "building code and docker image"
                sh "docker build -t my-flask-app ."
            }
        }
        stage("login and push") {
            steps {
                echo "dockerhub login and pushing the imahe to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass1",usernameVariable:"dockerhubUser1")]){
                    echo "Print dockerhub username ${env.dockerhubUser1}"
                    sh "docker tag my-flask-app ${env.dockerhubUser1}/my-flask-app:$BUILD_NUMBER"
                    sh "docker login -u ${env.dockerhubUser1} -p ${env.dockerhubPass1}"
                    sh "docker push ${env.dockerhubUser1}/my-flask-app:$BUILD_NUMBER"
                }
            }
        }
        stage("deploy") {
            steps {
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
