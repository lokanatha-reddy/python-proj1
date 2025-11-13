pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/lokanatha-reddy/python-proj1.git'
            }
        }

        stage('Install Dependencies') {
            steps {
               sh '''
            python3 -m venv venv
            . venv/bin/activate
            python3 -m pip install --upgrade pip
            pip install -r requirements.txt || true
        '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''#!/bin/bash
        source venv/bin/activate
        pytest
         '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t calc-app .'
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker tag calc-app $USER/calc-app:latest'
                    sh 'docker push $USER/calc-app:latest'
                }
            }
        }
    }
}
