pipeline {
    agent any
    
    tools {
        maven 'M3'
        jdk 'JDK8'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-repo/java-project.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        success {
            mail to: 'team@example.com',
                 subject: "Build Successful: ${currentBuild.fullDisplayName}",
                 body: "The build ${env.BUILD_URL} completed successfully."
        }
        failure {
            mail to: 'team@example.com',
                 subject: "Build Failed: ${currentBuild.fullDisplayName}",
                 body: "The build ${env.BUILD_URL} failed."
        }
    }
}