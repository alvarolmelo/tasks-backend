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
                withSonarQubeEnv('SONAR_LOCAL_QG'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=sqa_1e5b8f9a2f3c9a955d8eb1ef5693cfe5b4c95e97 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/text/**,**/model/**,**Aplication.java"
                }
            }
        }
        stage('Quality Gate'){
            steps{
                sleep(30)
                timeout(time: 1, unit: 'MINUTES'){
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

