pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        stage('Print Env Vars') {
            steps {
                echo "Artifact ID: '{$ArtifactId}'"
                echo "Version: '{$Version}'"
                echo "Name: '{$Name}'"
                echo "Group ID: '{$GroupId}'"
            }
        }

        // Nexus

        stage('Publish to Nexus') {
            steps {
                script {
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "Lab-SNAPSHOT" : "Lab-RELEASE"
                    
                    nexusArtifactUploader artifacts: [
                        [
                            // Use the variable for the artifact ID
                            artifactId: "${ArtifactId}", 
                        
                            classifier: '', 
                        
                            // Build the file path dynamically from the variables
                            file: "target/${ArtifactId}-${Version}.war", 

                            type: 'war'
                        ]
                    ], 
                    credentialsId: '7d196d2f-f3c1-4803-bde9-2d17d18776b3', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.10.25:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"
                }
            }
        }



        // Stage3 : Publish the source code to Sonarqube
        stage ('Deploy'){
            steps {
                // echo ' Source code published to Sonarqube for SCA......'
                // withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                //      sh 'mvn sonar:sonar'
                // }

                echo ' deploying......'

            }
        }

        
        
    }

}
