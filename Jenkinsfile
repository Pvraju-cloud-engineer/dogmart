pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Environment') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no \
                target/dogmart.war \
                ec2-user@172.31.22.8:/opt/tomcat/webapps/
                '''
            }
        }
    }

    post {
        success {
            echo 'DogMart deployed successfully!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}