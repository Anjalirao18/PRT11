pipeline {

    agent {
        label 'ci-agent'
    }

    stages {

        stage('Pull Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t prt-cicd:latest .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh 'sudo docker images prt-cicd'
                echo 'PRT - CI/CD Completed Successfully'
            }
        }

         stage('Run Docker Container') {
            steps {
                sh 'sudo docker run -d --name ubuntu-web -p 80:80 prt-cicd:latest'
            }
        }
    }   
     
 post {
       success {
           echo 'PRT - CI/CD Completed Successfully'
            }
        }
    }
