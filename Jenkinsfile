pipeline {
    agent any

    environment {
        SSH_KEY = credentials('my_key')  // Set your SSH key credential ID here
        REMOTE_HOST = '192.168.0.107'
        REMOTE_USER = 'mkk'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building Docker image...'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Docker container via SSH...'

                    def sshCommand = """
                        ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${REMOTE_USER}@${REMOTE_HOST} '
                            docker pull mohammed5253/task:hello &&
                            docker stop hello-web || true &&
                            docker rm hello-web || true &&
                            docker run -d -p 5000:80 --name hello-web mohammed5253/task:hello
                        '
                    """

                    def result = sh(script: sshCommand, returnStatus: true)

                    if (result == 0) {
                        echo 'SSH connection and deployment successful!'
                    } else {
                        error 'Failed to SSH and deploy the container.'
                    }
                }
            }
        }
    }
}
