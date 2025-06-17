pipeline {
    agent {
        docker {
            image 'docker/compose:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock -v $PWD:$PWD -w $PWD'
        }
    }

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
        SERVICE_NAME = 'wg-easy'
        CONTAINER_NAME = 'Wireguard'
    }

    stages {
        stage('Clonar Repositório') {
            steps {
                git 'https://github.com/guilhermeoliveirac/wireguard.git'
            }
        }

        stage('Subir com docker-compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Testar Container') {
            steps {
                script {
                    // Checar se o container está rodando
                    sh "docker ps | grep ${CONTAINER_NAME}"

                    // Exemplo: testar se a porta do painel web (51821) responde
                    sh "curl -s http://localhost:51821 || echo 'Porta 51821 não respondeu'"
                }
            }
        }

        stage('Encerrar Container') {
            steps {
                sh 'docker-compose down'
            }
        }
    }

    post {
        always {
            echo "Finalizando pipeline e limpando recursos..."
            sh 'docker-compose down || true'
        }
    }
}
