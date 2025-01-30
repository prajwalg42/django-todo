pipeline {
    agent any
    environment {
        KUBECONFIG = '/home/prajwal/.kube/config'  // Set the KUBECONFIG environment variable
    }
    stages {
        stage("Clone Repository") {
            steps {
                sh "git clone https://github.com/prajwalg42/django-todo.git"
            }
        }
        stage("Build Docker Image") {
            steps {
                dir('django-todo') {  // Navigate into the 'django-todo' directory
                    sh "docker build -t prajwalg42/todoapp:v2 ."
                }
            }
        }
        stage("Push Docker Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    dir('django-todo') {  // Navigate into the 'django-todo' directory
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                        sh "docker push prajwalg42/todoapp:v2"
                    }
                }
            }
        }
        stage("Deploy Kubernetes Resources") {
            steps {
                dir('django-todo') {  // Navigate into the 'django-todo' directory
                    sh "kubectl apply -f deploy.yml"
                    sh "kubectl apply -f service.yml"
                }
            }
        }
    }
}
