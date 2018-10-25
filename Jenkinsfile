pipeline{
    agent any
    stages{
        stage('Checkout'){
            steps{
                checkout scm
            }
        }          
        //Building the web-app
        stage('Build web-app'){
            steps{
                sh 'mvn -f /home/robot/.jenkins/workspace/test-jenkins-pipeline/hoteljsf clean install'
            }
        }
         //Building the web-service
        stage('Build web-service'){
            steps{
                sh 'mvn -f /home/robot/.jenkins/workspace/test-jenkins-pipeline/hotel-restfull clean install'
            }
        }
        //Deploy the artifacts on glassfish
        stage('Deploy artifacts'){
            steps{
                sh 'asadmin deploy --force=true /home/robot/.jenkins/workspace/test-jenkins-pipeline/hotel-restfull/target/hotel-rest.war'
                sh 'asadmin deploy --force=true /home/robot/.jenkins/workspace/test-jenkins-pipeline/hoteljsf/target/hotel.war'
            }
        }
        
        //Clean database
        stage('Clean database'){
            steps{
               sh 'psql -h localhost -U postgres hotel -f /home/robot/.jenkins/workspace/test-jenkins-pipeline/hoteljsf/database-backup.sql'
            }
        }
        
        //Frontend tests
        stage('Frontend tests'){
            steps{
               sh 'robot -d out-frontent --output output-frontend.xml /home/robot/.jenkins/workspace/test-jenkins-pipeline/robotframework-frontend/00_regression_tests.robot'
            }
        }
    }
}

