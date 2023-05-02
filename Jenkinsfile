pipeline {
    agent any
    environment {
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage ("code"){
            steps {
                git url: 'https://github.com/Chaitannyaa/django-todo-cicd.git', branch: 'develop'
            }
        }
        stage ("Build"){
            steps {
                sh 'docker build -t chaitannyaa/django-todo-app:v-${DOCKER_IMAGE_TAG} .'
            }
        }
        stage ("login and push image"){
            steps {
                echo 'Logging into docker and pushing an image on dockerhub'
                withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'password', usernameVariable: 'user')]) {
                    sh "docker login -u ${env.user} -p ${env.password}"
                    sh "docker push chaitannyaa/django-todo-app:v-${DOCKER_IMAGE_TAG}"
                }
            }
        }
        stage ("Deploy"){
            steps {
                if [ -f /home/ubuntu/db.sqlite3 ]; then
	            echo "Perfect"
                echo '1180' | sudo -S rm -f /var/lib/jenkins/workspace/django-todo-cicd/db.sqlite3
                echo '1180' | sudo -S cp -r /home/ubuntu/db.sqlite3 /var/lib/jenkins/workspace/django-todo-cicd/ 
                else
	            echo '1180' | sudo -S cp -r /var/lib/jenkins/workspace/django-todo-cicd/db.sqlite3 /home/ubuntu/db.sqlite3
                fi
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
