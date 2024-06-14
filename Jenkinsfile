pipeline {
    agent any

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
        stage("Nexus") {
            steps {
                nexusArtifactUploader artifacts: 
                [[artifactId: 'my-app',
                 classifier: '',
                  file: 'target/my-app-1.2.jar',
                   type: 'jar']], 
                    groupId: 'com.mycompany.app',
                     nexusUrl: 'http://192.168.56.201:8081/',
                      nexusVersion: 'nexus3', 
                      protocol: 'http',
                       repository: 'newrepo',
                       credentialsId: 'nexus_pwd',
                        version: '1.2'
            }
        }

    }
}
