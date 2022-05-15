pipeline {
    
	agent any
	
	tools {
        maven "maven"
    }
	
    environment {
       NEXUS_VERSION = "nexus3"
       NEXUS_PROTOCOL = "http"
       NEXUS_URL="172.31.15.189:8081"
       NEXUS_REPOSITORY = "release"
       NEXUS_REPOGRP_ID    = "maven-group"
       NEXUS_CREDENTIAL_ID = "nexus"
       ARTVERSION = "${env.BUILD_ID}"
       
       scannerHome= tool "sonarqube4.7.0.2747"

       
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

	    
	    stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                        def mavenPom = readMavenPom: 'pom.xml'
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: NEXUS_REPOGRP_ID,
                            version: ARTVERSION,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: 'chkmt',
                                classifier: '',
				 file: "target/chkmt-${mavenPom.version}.war",
                                type: 'war'
                              
                            ]
		          ]	    
                        );
                    } 
		  
                }
            }
        


    }


}
