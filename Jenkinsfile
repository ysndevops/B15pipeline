pipeline{
    agent{label 'node1'}
    tools{maven 'mvn-3.0.5'}
    stages{
        stage('PIPELINE JOB'){
            steps{
                echo "welcome to JENKINS PIPELINE JOB"
            }
        }
        stage('Checkout'){
            steps{
                echo "code checkout from git REPO"
                git branch: 'RELEASE_1.0.0', credentialsId: 'git', url: 'https://github.com/ysndevops/mvn2.git'
            }
        }
        stage('BUILD'){
            steps{
                echo "build the code using mvn-3.0.5"
                sh 'mvn package -f pom.xml'
            }
        }
        stage('DEV Deployment'){
            steps{
                echo "deploying war to DEV environment"
                deploy adapters: [tomcat9(credentialsId: 'tom9', path: '', url: 'http://34.16.209.56:8080')], contextPath: null, war: 'target/macys.war'
            }
        }
        stage('Approval to Deployment in STAGE env '){
            steps{
                echo "Taking approval from Manager to deploy WAR"
                timeout(time: 7, unit: 'DAYS') {
                input message: 'Do you want to deploy  WAR in STAGE env?', submitter: 'devops'
            }
            }
        }
        stage('STAGE env Deployment'){
            steps{
                echo "deploying on STAGE env"
                deploy adapters: [tomcat9(credentialsId: 'tom9', path: '', url: 'http://34.131.129.76:8090/')], contextPath: null, war: 'target/macys.war'
            }
            
        }
        stage('Approval to Deployment in PRE-PROD env '){
            steps{
                echo "Taking approval from Manager to deploy WAR "
                timeout(time: 7, unit: 'DAYS') {
                input message: 'Do you want to deploy  WAR ON PRE PROD env?', submitter: 'devops'
            }
            }
        }
        stage('PRE-PROD env Deployment'){
            steps{
                echo "deploying on PRE-PROD env"
                deploy adapters: [tomcat9(credentialsId: 'tom9', path: '', url: 'http://34.131.129.76:8080/')], contextPath: null, war: 'target/macys.war'
            }
            
        }
        stage('Approval to NEXUS REPO '){
            steps{
                echo "Taking approval from Manager for NEXUS REPO artifact push "
                timeout(time: 7, unit: 'DAYS') {
                input message: 'Do you want to deploy  WAR ON PRE PROD env?', submitter: 'devops'
            }
            }
        }
        stage('NESUS REPO'){
            steps{
                echo "calling NEXUS Certified job"
                build job: 'NEXUS-Certfied', parameters: [string(name: 'branch', value: 'RELEASE_1.0.0')]
            }
        }
    }
}
