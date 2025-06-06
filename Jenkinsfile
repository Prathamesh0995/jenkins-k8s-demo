pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'prathamesh1809/jenkins-k8s-demo'
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
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
                        script: "KUBECONFIG=${env.KUBECONFIG} kubectl config get-contexts -o name | grep -w minikube || true",
                        returnStdout: true
                    ).trim()
                    
                    if (!contextExists) {
                        error "❌ Kubernetes context 'minikube' not found or not accessible by Jenkins. Make sure KUBECONFIG is correct and has proper permissions."
                    } else {
                        echo "✅ Kubernetes context 'minikube' found."
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        export KUBECONFIG=${env.KUBECONFIG}
                        kubectl config use-context minikube
                        kubectl apply -f k8s/deployment.yaml
                    """
                }
            }
        }
    }
}
