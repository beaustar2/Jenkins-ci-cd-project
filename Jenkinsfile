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
                    echo 'Deploying....'
            sshagent(['Deploy_user']) {
            sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/jenkins-pipe/target/webapp-0.2.war centos@35.89.64.237:/home/centos/opt/tomcat"
            }
        }
    }
}
