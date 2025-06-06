pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'prathamesh1809/jenkins-k8s-demo'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Prathamesh0995/jenkins-k8s-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([ credentialsId: 'dockerhub-creds', url: '' ]) {
                    script {
                        docker.image(DOCKER_IMAGE).push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl config use-context minikube
                kubectl apply -f k8s/deployment.yaml
                '''
            }
        }
    }
}

