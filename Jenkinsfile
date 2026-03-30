pipeline {
    agent any
    stages{

        stage('git cloned'){
            steps{
                git url:'https://github.com/Pooji2121/php-project/', branch: "master"
            }
        }

        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t pooji09/5sepimage:v1 .'
                    sh 'docker images'
                }
            }
        }

        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push pooji09/5sepimage:v1'
                }
            }
        }
        
        stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                   def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 pooji09/5sepimage:v1'

                    sshagent(['sshkeypair']) {
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.77 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.77 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
