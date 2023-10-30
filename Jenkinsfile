pipeline {
    agent any

    stages {
        stage('Build') {
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo"Archiving the Artifacts"
                    archiveArtifacts artifacts: '*/target/.war'
                }
            }
            steps {
                sh 'echo "Building the project"'
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests"'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying the application"'
            }
        }
    }
