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
                withDockerRegistry([credentialsId: 'edb56442-75ba-4f05-92b7-b5b5088aa113', url: '']) {
                    script {
                        docker.image(DOCKER_IMAGE).push('latest')
                    }
                }
            }
        }

        stage('Validate Kubernetes Context') {
            steps {
                script {
                    def contextExists = sh(
                        script: "kubectl config get-contexts -o name | grep -w minikube || true",
                        returnStdout: true
                    ).trim()
                    
                    if (!contextExists) {
                        error "Kubernetes context 'minikube' not found. Please run 'minikube start' and ensure the kubeconfig is accessible."
                    } else {
                        echo "✅ Kubernetes context 'minikube' found."
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                        export KUBECONFIG=/home/prathamesh/.kube/config
                        sudo -E kubectl config use-context minikube
                        sudo -E kubectl apply -f k8s/deployment.yaml
                    '''
                }
            }
        }
    } // <-- This was missing
}
