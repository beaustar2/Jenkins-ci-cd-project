pipeline {
    agent any
    environment {
        PROJECT_PATH = '/home/centos/Jenkins-ci-cd-project/src/test/java/mypackage'
        MAVEN_PATH = 'usr/bin/mvn'
    }
    stages {
        stage('Checkout') {
            steps {
                sh 'echo passed'
                // git branch: 'master', url: https://github.com/beaustar2/Jenkins-ci-cd-project.git
                checkout scm
            }
        }
        stage('Build') {
            agent any
            steps {
                echo 'Building in progress'
                sh 'ls -ltr'
            }
        }
        stage('Build and Package') {
            steps {
                script {
                    // Change directory and run Maven commands
                    sh """
                        cd \${PROJECT_PATH}
                        \${MAVEN_PATH} clean package
                    """
                }
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                // Deploy your application
                sh 'scp /home/centos/Jenkins-ci-cd-project/Jenkinsfile centos@172.31.6.181:/home/centos/apache-tomcat-7.0.94/webapps/WebAppCal-0.0.6/WEB-INF/Jenkinsfile'
            }
            post {
                success {
                    script {
                        // send email for successful build
                        mail to: 'kelvinatete@yahoo.com',
                             subject: "Build Successful - ${currentBuild.fullDisplayName}",
                             body: "Congratulations! The build was successful.\n\nCheck console output at ${BUILD_URL}"
                    }
                }
                failure {
                    script {
                        // Send email for failed build
                        mail to: 'kelvinatete@yahoo.com',
                             subject: "Build Failed - ${currentBuild.fullDisplayName}",
                             body: "The build was unsuccessful.\n\nCheck console output at ${BUILD_URL}"
                    }
                }
            }
        }
    }
}
