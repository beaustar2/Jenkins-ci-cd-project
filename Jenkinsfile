pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building the project"'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests"'
            }
        }
        stage('deploy') {
            steps {
                sh 'echo "Deploying...."'    
                sshagent(['centos']) {
                    sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Jenkins Pipeline Project/target/webapp-0.2.war centos@3.135.192.169:/home/centos/opt/tomcat'
                }
            }
        }
    }
}
