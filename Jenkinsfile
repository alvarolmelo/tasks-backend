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
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=sqa_1e5b8f9a2f3c9a955d8eb1ef5693cfe5b4c95e97 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**, **/src/text/**,**/model/**,**Aplication.java"
                }
            }
        }


    }
}

