pipeline {
    //Directives
    agent any
    tools {
        maven 'Maven'
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
        stage('Build') {
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage('Test') {
            steps {
                echo ' testing......'
            }
        }

        // Stage 3 : Publish the artifacts to nexus

        stage('Publish to Nexus') {
            steps {
                script {
                    def NexusRepo = Version.endsWith('SNAPSHOT') ? 'KKDevOpsLab-SNAPSHOT' : 'KKDevOpsLab-RELEASE'
                    nexusArtifactUploader artifacts:
                 [[artifactId: "${ArtifactId}",
                  classifier: '',
                   file: "target/${Name}-${Version}.war",
                    type: 'WAR']],
                     credentialsId: 'd870f477-b1a0-4dc2-a28b-a5995d9f75b6',
                      groupId: "${GroupId}",
                       nexusUrl: '3.236.139.208:8081',
                        nexusVersion: 'nexus3',
                         protocol: 'http',
                          repository: "${NexusRepo}",
                           version: "${Version}"
                }
            }
        }

        // Stage 5 : Publish the source code to Sonarqube
        stage('Deploy') {
            steps {
                echo ' deploying.....'
            }
        }
    }
}
