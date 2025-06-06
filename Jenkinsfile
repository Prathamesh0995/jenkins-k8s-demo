pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'prathamesh1809/jenkins-k8s-demo:latest'
        KUBECONFIG = '/home/prathamesh/.kube/config'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Prathamesh0995/jenkins-k8s-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials', url: 'https://index.docker.io/v1/']) {
                    script {
                        sh '''
                            docker tag $DOCKER_IMAGE $DOCKER_IMAGE
                            docker push $DOCKER_IMAGE
                        '''
                    }
                }
            }
        }

        stage('Validate Kubernetes Context') {
            steps {
                script {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG
                        if kubectl config get-contexts -o name | grep -w minikube; then
                            echo "✅ Kubernetes context 'minikube' found."
                        else
                            echo "❌ Minikube context not found." && exit 1
                        fi
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
<<<<<<< HEAD
                script {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG
                        sudo -E kubectl config use-context minikube
                        sudo -E kubectl apply -f k8s/deployment.yaml
                    '''
                }
=======
                sh '''
                kubectl config use-context minikube
                kubectl apply -f k8s/deployment.yaml
                '''
>>>>>>> parent of 06e3e5c (Update Jenkinsfile)
            }
        }
    }
}
<<<<<<< HEAD
=======

>>>>>>> parent of 06e3e5c (Update Jenkinsfile)
