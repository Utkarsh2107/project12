pipeline {
   
    agent any
   
    tools {
       
        maven 'Maven-Tool'
       
    }
   
    environment {
           
            IP_ADDRESS = "172.31.89.210"
        }
   
    stages {
       
        stage ("Git Checkout") {
           
            steps {
               
                git url: 'https://github.com/Shahid199578/firstrepo.git'
               
            }
           
        }
       
        stage ("Maven Build") {
           
            steps {
               
                sh "mvn clean package"
               
            }
        }
       
        stage ("Upload War File") {
           
            steps {
                sh 'sudo cp target/*.war /opt/apache-tomcat-9.0.76/webapps'               
                          
            }           
        }
       
        stage ("Upload-Artifact") {
           
            steps {
               
                script {
                   
                    def mavenPom = readMavenPom file: 'pom.xml'
                   
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: "${mavenPom.artifactId}",
                            classifier: '',
                            file: "target/${mavenPom.artifactId}.war",
                            type: "${mavenPom.packaging}"
                        ]
                    ],
                        credentialsId: 'nexus3',
                        groupId: "${mavenPom.groupId}",
                        nexusUrl: "${env.IP_ADDRESS}:8081",
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        repository: 'Nexus-Pipeline/',
                        version: "${mavenPom.version}"                   
                }
               
            }
           
        }       
    }
}
