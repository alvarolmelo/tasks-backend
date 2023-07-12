pipeline{
    agent any
    stages {
        stage('Build Backend'){
            steps{
                bat 'mvnw clean package -DskipTests=true'
            }
        }
        stage('Test Backend'){
            steps{
                bat 'mvnw clean test'
            }
        }


    }
}