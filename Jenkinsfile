pipeline {
    agent any
   
    stages {
        stage('Clone Repo') {
            steps {
                echo 'Not required as git clone is handled by Jenkins directly'
                //git url: 'https://github.com/timchristopher/duo-task', branch: 'master'
            }
        }
        stage('Pre-build Cleanup') {
            steps {
                echo 'Clean-up'
                // sh 'docker system prune -f'
                sh 'docker stop duo-task && docker rm duo-task || docker rm duo-task'
            }
        }
        stage('Build and Run') {
            steps {
                echo 'Build'
                sh '''
                docker network inspect duo-net && sleep 1 || docker network create duo-net
                docker build -t duo-task:v1 .
                docker run -d --network duo-net -p 5000:5500 --name duo-task duo-task:v1
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Test'
            }
        }
        stage('Remote stage') {
            steps {
                echo 'Remote test'
            }
        }
    }

}
