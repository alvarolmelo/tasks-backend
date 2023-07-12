pipeline{
    agent any
    stages {
        stage('Build Backend'){
            steps{
                bat 'mvnw clean package -DskipTests=true'
            }
        }
        stage('Unit Tests'){
            steps{
                bat 'mvnw test'
            }
        }
        stage('Sonar Alalysis'){
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=sqp_f9e31f0d4ec13f2565bc42a9c7a2adea0b4eb86b -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/text/**,**/model/**,**Aplication.java"
                }
            }
        }
        stage('Quality Gate'){
            steps{
                sleep(30)
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy Backend'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/manager')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }


    }
}

