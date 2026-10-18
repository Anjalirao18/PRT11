
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
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    sudo docker rm -f ubuntu-web 2>/dev/null || true

                    sudo docker run -d \
                        --name ubuntu-web \
                        -p 80:80 \
                        prt-cicd:latest

                    sudo docker ps
                '''
            }
        }

        stage('Verify Webpage Output') {
            steps {
                sh '''
                    sleep 3
                    sudo docker exec ubuntu-web \
                        cat /usr/share/nginx/html/index.html

                    curl -fsS http://localhost \
                        | grep -F "PRT - CI/CD Completed Successfully"
                '''
            }
        }
    }

    post {
        success {
            echo 'PRT - CI/CD Completed Successfully'
        }
    }
}

