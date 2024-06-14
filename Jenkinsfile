pipeline {
    agent any
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://192.168.56.201:8081/"
        NEXUS_REPOSITORY = "newrepo"
        NEXUS_CREDENTIAL_ID = "nexus_pwd"
    }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven3"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main' , url: 'https://github.com/aymendr/projet-maven.git'
            }
        }
        stage('Build Maven') {
            steps {
                sh "mvn -DskipTests clean package"
               }
        }
        /*stage('SonarQube Analysis') {
            steps {
                // Execute SonarQube analysis
                script {
                    withSonarQubeEnv('sonar_server') {
                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    }
                }
            }
        } 
        stage('SonarQube Analysis') {
            steps {
                // Execute SonarQube analysis
                script {
                    def scannerHome = tool 'sonar_scanner_tool'
                    withSonarQubeEnv('sonarqubeserver') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=my-app \
                        -Dsonar.java.binaries=target/test/com/app/"
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 3, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }*/


        stage('Upload to Nexus') {
            steps {
                script {
                    // Définir le chemin vers l'artefact généré
                    def artifactPath = "target/my-app-1.2.jar"
                    def artifactGroup = "com.mycompany.app"
                    def artifactName = "my-app"
                    def artifactVersion = "1.2"

                    nexusArtifactUploader(
                        nexusVersion: NEXUS_VERSION,
                        protocol: NEXUS_PROTOCOL,
                        nexusUrl: NEXUS_URL,
                        groupId: artifactGroup,
                        version: artifactVersion,
                        repository: NEXUS_REPOSITORY,
                        credentialsId: NEXUS_CREDENTIAL_ID,
                        artifacts: [
                            [artifactId: artifactName,
                             classifier: '',
                             file: artifactPath,
                             type: 'jar']
                        ]
                    )
                }
            }
        }

    }
}
