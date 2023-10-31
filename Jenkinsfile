pipeline {
    agent any
    tools {
        // Define the Maven tool with the desired version
        maven 'Apache Maven 3.0.5'
    }
    stages {
        stage('Checkout') {
            steps {
                sh 'echo passed'
                // Checkout the source code from the Git repository
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
                sh 'cd Jenkins-ci-cd-project && mvn clean package'
            }
        }

        stage('Test') {
            agent any
            steps {
                // Run tests using Maven
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy your application using scp
                sh 'scp /home/centos/Jenkins-ci-cd-project/Jenkinsfile centos@172.31.6.181:/home/centos/apache-tomcat-7.0.94/webapps/WebAppCal-0.0.6/WEB-INF/Jenkinsfile'
            }
            post {
                success {
                    script {
                        // Send an email for a successful build
                        mail to: 'kelvinatete@yahoo.com',
                            subject: "Build Successful - ${currentBuild.fullDisplayName}",
                            body: "Congratulations! The build was successful.\n\nCheck console output at ${BUILD_URL}"
                    }
                }
                failure {
                    script {
                        // Send an email for a failed build
                        mail to: 'kelvinatete@yahoo.com',
                            subject: "Build Failed - ${currentBuild.fullDisplayName}",
                            body: "The build was unsuccessful.\n\nCheck console output at ${BUILD_URL}"
                    }
                }
            }
        }
    }
}
