pipeline {
    agent any

    stages {

        stage('Checkout Source') {
            steps {
                git url:'https://github.com/tmfonseca/pedelogo-catalogo.git', branch:'main'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("tmfonseca/api-produto:${env.BUILD_ID}",'-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
                }
            }
        }
        

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy Kubernetes') {
            enviroment {
                tag_version = "${env.BUIL_ID}"
            }
            steps {
                script {
                    sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/api/api-deploy.yaml'
                    sh 'cat ./k8s/api/api-deploy.yaml'
                    kubeconfig(credentialsId: 'kubeconfigfile', serverUrl: '') {
                        sh 'kubectl apply -f ./k8s/ -R'
                    }
                }
            }
        }
        

    }
}
