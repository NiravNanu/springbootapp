pipeline {
    agent any
    
    tools {
        maven "Maven3"
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        stage('Checkout from git') {
            steps {
                git branch: 'main', url: 'https://github.com/NiravNanu/springbootapp.git'
            }
        }
        
        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Unit Test') {
            steps {
                echo '<----------------------Unit Test Under Progess------------------>'
                sh 'mvn surefire-report:report'
                echo '<----------------------Unit Test Done------------------>'
            }
        }
        
        stage('Sonarqube analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=springbootapp -Dsonar.projectKey=springbootapp"
                }
            }
        }
        
        stage('Quality Gate Test') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, unstable: true
                }
            }
        }
    }
}

