pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Clean up existing Docker container and image
                    sh 'docker stop hello-web || true'
                    sh 'docker rm hello-web || true'
                    sh 'docker rmi mohammed5253/task:hello || true'

                    // Build the Docker image
                    sh 'docker build -t mohammed5253/task:hello .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Run the Docker container on the remote server using SSH
                    sshagent(['your-ssh-key-id']) {
                        sh 'ssh -o StrictHostKeyChecking=no -l mkk 192.168.0.107 "docker pull mohammed5253/task:hello"'
                        sh 'ssh -o StrictHostKeyChecking=no -l mkk 192.168.0.107 "docker stop hello-web || true"'
                        sh 'ssh -o StrictHostKeyChecking=no -l mkk 192.168.0.107 "docker rm hello-web || true"'
                        sh 'ssh -o StrictHostKeyChecking=no -l mkk 192.168.0.107 "docker run -d -p 5000:80 --name hello-web mohammed5253/task:hello"'
                    }
                }
            }
        }
    }
}
