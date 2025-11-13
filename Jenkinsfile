pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/lokanatha-reddy/python-proj1.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // install requirements from the repo
                sh 'python -m pip install --upgrade pip'
                sh 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // run pytest; fail build if tests fail
                sh 'pytest --maxfail=1 --disable-warnings -q'
            }
        }

        stage('Build Docker Image') {
            steps {
                // build docker image locally
                sh 'docker build -t calc-app .'
            }
        }

        stage('Docker Login & Push') {
            steps {
                // login to docker hub using credentials stored in Jenkins
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo "Logging in to Docker Hub..."'
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker tag calc-app $USER/calc-app:latest'
                    sh 'docker push $USER/calc-app:latest'
                }
            }
        }
    }
}
