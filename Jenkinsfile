pipeline {
    agent any
    tools {
        // Define the 'Maven' tool for use in this pipeline
        maven 'Maven-3.6.3'
    }
    stages {
        stage('Checkout') {
            steps {
                sh 'echo passed'
                checkout scm
            }
        }

        stage('Build') {
            agent {
                label 'node1'
            }
            steps {
                echo 'Building in progress'
                sh 'ls -ltr'
                // Build the project and create a JAR file
                sh 'mvn clean package' // Fixed the build command
            }
        }

        stage('Test') {
            agent {
                label 'node1'
            }
            steps {
                // Run tests
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            agent {
                label 'Node2'
            }
            steps {
                // Deploy your application
                sh 'scp /home/centos/Jenkins-ci-cd-project/Jenkinsfile centos@172.31.6.181:/home/centos/apache-tomcat-7.0.94/webapps/WebAppCal-0.0.6/WEB-INF/Jenkinsfile'
            }

            post {
                success {
                    script {
                        // Send email for successful build
                        mail to: 'kelvinatete@yahoo.com',
                             subject: "Build Successful - ${currentBuild.fullDisplayName}",
                             body: "Congratulations! The build was successful.\n\nCheck console output at ${BUILD_URL}"
                    }
                }
                failure {
                    script {
                        // Send email for a failed build
                        mail to: 'kelvinatete@yahoo.com',
                             subject: "Build Failed - ${currentBuild.fullDisplayName}",
                             body: "Oops! The build failed.\n\nCheck console output at ${BUILD_URL}"
                    }
                }
            }
        }
    }
}
