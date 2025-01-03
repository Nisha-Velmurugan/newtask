pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Ensure 'sonarmaven' matches the Maven installation name in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the Git repository
                git branch: 'main', url: 'https://github.com/Nisha-Velmurugan/newtask.git'
            }
        }
        stage('Build and Test') {
            steps {
                // Clean the project and run tests
                sh 'mvn clean test'
            }
        }
        stage('Code Coverage') {
            steps {
                // Generate the JaCoCo code coverage report
                sh 'mvn jacoco:report'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with the name of your SonarQube configuration in Jenkins
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=newtask \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=sqp_7121bab83e40061e0df2f6f9e13c976639dbcfd8
                    '''
                }
            }
        }
    }
    post {
        always {
            // Publish JUnit test results
            junit '**/target/surefire-reports/*.xml'
            
            // Archive any artifacts (optional)
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}
