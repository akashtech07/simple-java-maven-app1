pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/akashtech07/simple-java-maven-app1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=akash-org_project \
                        -Dsonar.organization=akash-org \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build Successful ✔'
        }
        failure {
            echo 'Build Failed ❌'
        }
    }
}
