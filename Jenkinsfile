pipeline{
    agent any

    stages {
        // Primera etapa.- Parar los servicios
        stage("Stopping Services") {
            steps{
                bat '''
                    docker compose -p adj-demo down
                '''
            }
        }

        //Eliminando
        stage("Deleting Services") {
            steps{
                bat '''
                    IMAGES=$(docker images rmi --filter "label=com.docker.compose.project=adj-demo" -q)
                    if [-n '$IMAGES']; then
                        docker images rmi $IMAGES
                    else
                        echo 'No hay imagenes por borrar..'
                    fi
                '''
            }
        }
        
        //Actualizando
        stage("Refresing project") {
            steps{
                checkout scm
            }
        }
        
        //Construccion
        stage("Get up Services") {
            steps{
                bat '''
                    docker compose up --build -d
                '''
            }
        }

    
    }

    post {
        success{
            echo 'Pipeline ejecutada exitosamente'
        }

        failure{
            echo 'Error ejecutando el pipeline'
        }

        always{
            echo 'Pipeline finalizado'
        }

    }


}