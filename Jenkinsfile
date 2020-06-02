pipeline {
    agent any
    environment {
        docker_host = "10.1.1.225"
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/vinnusmiley/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'docker build -t vinnuvikki/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push vinnuvikki/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://$docker_host:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://$docker_host:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 vinnuvikki/pipelinetestprod:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://$docker_host:8000'
          }
        }

    }
  }
}
