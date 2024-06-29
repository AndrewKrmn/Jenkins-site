pipeline {
    agent any

    

    stages {
        stage('Create Network') {
            steps {
                script {
                    sh """
                    docker network rm -f site || true
                    docker network create site
                    """
                }
            }
        }
        stage('Run MSSQL Container') {
            steps {
                script {
                    sh """
                    docker rm -f mssql
                    docker run -d -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Qwerty-1" --name mssql --network site -p 1433:1433 --restart unless-stopped mcr.microsoft.com/mssql/server:2022-latest
                    """
                }
            }
        }
        stage('Run Frontend Container') {
            steps {
                script {
                    sh """
                    docker rm -f frontend
                    docker run -d --name frontend --network site -p 80:80 --restart unless-stopped chikibevchik/front
                    """
                }
            }
        }
        stage('Run Backend Container') {
            steps {
                script {
                    sh """
                    docker rm -f backend
                    docker run -d --name backend --network site -p 5034:5034 --restart unless-stopped chikibevchik/back
                    """
                }
            }
        }
    }
}
