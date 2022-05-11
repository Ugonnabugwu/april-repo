pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/Ugonnabugwu/april-repo.git'
            }
        }
    
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('mysonar')  {
                        sh 'mvn -f AprilApp/pom.xml clean package sonar:sonar'
                    }
                }
            }
        
     stage('Unit Test') {
            steps {
                sh "cd AprilApp && mvn test"
            }
        }
        
        stage('Build with maven') {
            steps {
                sh "cd AprilApp && mvn package"
            }
        }
        
        stage('Deploy to tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tom-cat', path: '', 
                url: 'http://18.221.30.32:8080')], 
                contextPath: 'nnpcpipeline', war: '**/*.war'
            }
        }
    }
}
