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
                sh 'docker ps -q --filter "name=^duo-task\$" | grep -q . && docker stop duo-task | (echo -n "Stopped " && cat) || echo "Application \'duo-task\' is not running"'
                sh 'docker ps -qa --filter "name=^duo-task\$" | grep -q . && docker rm duo-task | (echo -n "Removed container " && cat) || echo "Container named \'duo-task\' does not exist"'
                sh 'docker network ls -q --filter "name=^duo-net\$" | grep -q . && docker network rm duo-net || echo "duo-net does not exist"'
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
    }

}
