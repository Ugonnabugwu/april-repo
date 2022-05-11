pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/Ugonnabugwu/april-repo.git'
            }
        }
        
        stage('Test') {
            steps {
                sh 'cd AprilApp && mvn test'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd AprilApp && mvn clean package'
            }
        }

        stage('Deploy to Staging') {
            when {
               branch "staging"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-2', applicationName: 'Tomcat-app', environmentName: 'staging-env', keyPrefix: 'stage', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-2-697102533223', rootObject: 'AprilApp/target/AprilApp-1.0.null.war', versionLabelFormat: 'staging-$BUILD_NUMBER'])
                
            }
        }

        stage('Deploy to Prod') {
             when {
               branch "master"
                }
            steps {
                step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-cred',
                awsRegion: 'us-east-2', applicationName: 'Tomcat-app', environmentName: 'production-env', keyPrefix: 'prod', sleepTime: 5,
                bucketName: 'elasticbeanstalk-us-east-2-697102533223', rootObject: 'AprilApp/target/AprilApp-1.0.null.war', versionLabelFormat: 'prod-$BUILD_NUMBER'])
                
            }
        }
    }
}
