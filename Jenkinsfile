pipeline {
    agent any
    tools {
        maven 'Maven'  // Make sure this matches the name in Jenkins tools
        jdk 'java21'   // The JDK you configured in Jenkins
    }
    stages {
        stage('CloneRepo') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/himanshumahtolia/DevOpsClassCodes.git'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling code...'
                bat 'mvn compile'
            }
        }

        stage('CodeReview') {
            steps {
                echo 'Running PMD analysis...'
                bat 'mvn pmd:pmd'
            }
            post {
                success {
                    recordIssues(tools: [pmdParser(pattern: '**/pmd.xml')])
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging...'
                bat 'mvn package'
            }
        }
    }
}
