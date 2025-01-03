pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Ensure Maven is configured in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Nisha-Velmurugan/newtask.git'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=newtask -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_7121bab83e40061e0df2f6f9e13c976639dbcfd8'
                }
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
