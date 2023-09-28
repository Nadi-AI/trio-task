pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                sh '''
                echo "Hello, welcome to my Jenkins"
                '''
            }
        }
        stage('Pull repo') {
            steps {
                sh '''
                echo "The next step"
                '''
            }
        }
        stage('Flask-app triggers') {
            steps {
                sh '''
                docker build -t flask-app
                '''
            }
        }
        stage('Database triggers') {
            steps {
            sh '''
            docker build -t mysql
            '''
            }
        }
        stage('NGINX tings') {
            steps {
            sh '''
            docker build -t nginx-app
            docker run -d -p 80:80 nginx-app
            '''
            }
        }
                stage('Networks') {
            steps {
            sh '''
            Hostname
            '''
            }
        }
    }
}