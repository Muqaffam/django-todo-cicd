pipeline {
    agent any
    environment {
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage ("code"){
            steps {
                git url: 'https://github.com/Muqaffam/django-todo-cicd.git', branch: 'develop'
            }
        }
        stage ("Build"){
            steps {
                sh 'docker build -t Muqaffam/django-todo-cicd:v-${DOCKER_IMAGE_TAG} .'
            }
        }
        stage ("login and push image"){
            steps {
                echo 'Logging into docker and pushing an image on dockerhub'
                withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'password', usernameVariable: 'user')]) {
                    sh "docker login -u ${env.user} -p ${env.password}"
                    sh "docker push Muqaffam/django-todo-cicd:v-${DOCKER_IMAGE_TAG}"
                }
            }
        }
        stage("Deploy"){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
