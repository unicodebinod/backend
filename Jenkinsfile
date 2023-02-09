pipeline {
    agent {
        node {
            label 'webserver'
        }
    }
    stages {
        stage('Build') {
            steps {
              sh '''
                docker build -t abdulwahabshukri/jenkins-backend:jenkins-${GITHUB_RUN_ID} .
              '''
            }
        }
        stage('Release') {
            steps {
              withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker push abdulwahabshukri/jenkins-backend:jenkins-${GITHUB_RUN_ID}
                '''
              }
            }
        }
        stage('deploy') {
            steps {
                sh '''
                docker stop jenkins-backend || true
                docker rm -f jenkins-backend || true
                docker run -p5000:3000 -v /home/deploy/data.csv:/app/data.csv -d --name jenkins-backend abdulwahabshukri/jenkins-backend:jenkins-${GITHUB_RUN_ID}
                '''
            }
        }
    }
}