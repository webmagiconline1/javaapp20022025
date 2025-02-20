pipeline {
    agent any
    tools {
        maven 'Maven_3.9.9'  // The name you gave your Maven installation
    }

    stages {
        stage('Unit Test') {
            steps {
                sh 'mvn test'
                publishHTML(target: [
                    reportDir: 'target/surefire-reports',
                    reportFiles: 'index.html',
                    reportName: 'Unit Test Report'
                ])
            }
        }
        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
                publishHTML(target: [
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html',
                    reportName: 'Code Coverage Report'
                ])
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=thakur-devops-workshop_my-app -Dsonar.login=$SONAR_TOKEN'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
