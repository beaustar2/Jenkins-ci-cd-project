pipeline {
    agent any
    environment {
        PROJECT_PATH = "/home/centos/Jenkins-ci-cd-project"
        MAVEN_PATH = '/opt/maven/bin/mvn'
        JAVA_HOME = '/usr'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the source code'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building in progress'
                sh 'ls -ltr'
            }
        }
        stage('Build and Package') {
            agent { label 'node1' }
            steps {
                script {
                    // Change directory and run Maven commands
                    dir("${PROJECT_PATH}") {
                        sh """
                            ${MAVEN_PATH} clean package
                            export JAVA_HOME=${JAVA_HOME}
                            ${MAVEN_PATH} clean install --update-snapshots
                        """
                    }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests'
                sh "${MAVEN_PATH} test" // Use the MAVEN_PATH defined in the environment
            }
        }
        stage('Deploy') {
            agent { label 'node2' }
            steps {
                echo 'Deploying your application'
                sh "scp ${PROJECT_PATH}/Jenkinsfile centos@172.31.6.181:/home/centos/apache-tomcat-7.0.94/webapps/WebAppCal-0.0.6/WEB-INF/Jenkinsfile"
            }
            post {
                success {
                    script {
                        echo 'Sending an email for a successful build'
                        emailext (
                            to: 'kelvinatete@yahoo.com',
                            subject: "Build Successful - ${currentBuild.fullDisplayName}",
                            body: "Congratulations! The build was successful.\n\nCheck the console output at ${BUILD_URL}"
                        )
                    }
                }
                failure {
                    script {
                        echo 'Sending an email for a failed build'
                        emailext (
                            to: 'kelvinatete@yahoo.com',
                            subject: "Build Failed - ${currentBuild.fullDisplayName}",
                            body: "The build was unsuccessful.\n\nCheck the console output at ${BUILD_URL}"
                        )
                    }
                }
            }
        }
    }
}
