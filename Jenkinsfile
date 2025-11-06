pipeline {
    agent any

    stages {
        stage("Stopping Services") {
            steps {
                bat '''
                    echo Apagando servicios Docker...
                    docker compose -p adj-demo down
                '''
            }
        }

        stage("Deleting Services") {
            steps {
                bat '''
                    echo Buscando imágenes con etiqueta adj-demo...
                    for /f %%i in ('docker images --filter "label=com.docker.compose.project=adj-demo" -q') do (
                        docker rmi -f %%i
                    )
                    echo Imágenes eliminadas (si existían).
                '''
            }
        }

        stage("Refreshing Project") {
            steps {
                checkout scm
            }
        }

        stage("Get up Services") {
            steps {
                bat '''
                    echo Levantando servicios con Docker Compose...
                    docker compose -p adj-demo up --build -d
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline ejecutada exitosamente'
        }
        failure {
            echo '❌ Error ejecutando el pipeline'
        }
        always {
            echo '📦 Pipeline finalizado'
        }
    }
}