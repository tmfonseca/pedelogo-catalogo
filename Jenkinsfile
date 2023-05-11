pipeline {
    agent any

    stages {

        stage('Checkout Source') {
            steps {
                git url:'https://github.com/tmfonseca/pedelogo-catalogo.git', branch:'master'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("tmfonseca/api-produto:${env.BUILD_ID}",'-f ./src/Pedelogo.Catalogo.Api/Dockerfile .')
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



    }
}
