pipeline {
    agent any

    stages {
        stage('GIT CHECKOUT') {
            steps {
                git branch: 'main', url: 'https://github.com/vishalchauhan91196/java-code.git'
            }
        }

        stage('UNIT TESTING') {
            steps {
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TESTING') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage('MAVEN BUILD') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('STATIC CODE ANALYSIS') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqubetoken') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage('QUALITY GATE STATUS') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqubetoken'
                }
            }
        }

        stage('Upload Artifacts to Nexus'){
            
            steps{

                script{

                    nexusArtifactUploader artifacts: 
                [
                    [
                    artifactId: 'springboot',
                    classifier: '', 
                    file: 'target/Thapar.jar', 
                    type: 'jar'
                    ]
                ], 
                    credentialsId: 'nexuscreds', 
                    groupId: 'com.example', 
                    nexusUrl: '52.194.188.17:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'java-release', 
                    version: '1.0.0'
                }
            }
        }
    }
}