pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                sh '''
                echo "OHFFS"
                '''
            }
        }
        stage('Build images') {
            steps {
                sh '''
                echo "Hello, Starting the build process"

                docker build -t scribral/trio-task-db:latest ./db
                docker build -t scribral/trio-task-db:${BUILD_NUMBER} ./db

                docker build -t scribral/trio-task-app:latest ./flask-app
                docker build -t scribral/trio-task-app:${BUILD_NUMBER} ./flask-app

                docker build -t scribral/trio-task-rp:latest ./nginx
                docker build -t scribral/trio-task-rp:${BUILD_NUMBER} ./nginx
                '''
            }
        }
        stage('Push Images to DH') {
            steps {
                sh '''
               docker push scribral/trio-task-db
               docker push scribral/trio-task-db:${BUILD_NUMBER}
               docker push scribral/trio-task-app
               docker push scribral/trio-task-app:${BUILD_NUMBER}
               docker push scribral/trio-task-rp
               docker push scribral/trio-task-rp:${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy Containers to AppServ') {
            environment {
                MYSQL_ROOT_PASSWORD = credentials('MYSQL_ROOT_PASSWORD')
            }
            steps {
                sh '''
                ssh 10.154.0.30 << EOF
                docker pull scribral/trio-task-db
                docker pull scribral/trio-task-app
                docker network inspect trio-network && sleep 1 || docker network create trio-network
                docker volume inspect trio-network && sleep 1 || docker volume create trio-network
                docker stop mysql && (docker rm mysql || (docker rm mysql && sleep 1 || sleep 1)                    docker rm mysql
                docker stop flask-app && (docker rm flask-app || (docker rm flask-app && sleep 1 || sleep 1)
                docker stop nginx && (docker rm nginx || (docker rm nginx && sleep 1 || sleep 1)
                docker run -d -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} -v trio:/var/lib/mysql --nework trio --name mysql scribral/trio-task-db
                docker run -d -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} --nework trio --name flask-app scribral/trio-task-app
                docker run -d -e  --nework trio --name flask-app scribral/trio-task-app
                EOF
                '''
            }
        }
        stage('Cleanup Jenkins') {
            steps {
            sh '''
            docker rmi scribral/trio-task-db
            docker rmi scribral/trio-task-db:${BUILD_NUMBER}
            docker rmi scribral/trio-task-app
            docker rmi scribral/trio-task-app:${BUILD_NUMBER}
            docker rmi scribral/trio-task-rp
            docker rmi scribral/trio-task-rp:${BUILD_NUMBER}
            '''
            }
        }
    }
}