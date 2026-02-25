pipeline{
    agent any
    tools {maven "maven"}
     environment {
        RELEASE_REPO = 'petclinic'
        NEXUS_IP ='54.204.55.217'
        NEXUS_PORT ='8081'
        NEUXS_LOGIN ='neuxs'
        DOCKERHUB_CREDENTIALS = credentials('docker-tocken')
        REGISTRY = 'petclinic'
     } 
     stages{
        stage('git-clone'){
            steps{
                git branch: 'main', credentialsId: 'edc43311-5408-4645-8be5-df7162b7f887', url: 'https://github.com/nagadurga8/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Sanarqube'){
            steps{
                withSonarQubeEnv('sonarqube'){
                    sh 'maven clean verify sonar:sonar \
                    -Dsonar.projectkey = petclinic \
                    -Dsonar.login=sqp_9e200dbe6b081c0995427414f0fac96bd132017e'
                }
            }
        }
        stage('Update jar file to Nexes'){
            steps{
                nexusArtifactUploader(
                    nexusVersion : 'Nexus3',
                    protocol : 'http',
                    nexusUrl :"${NEXUS_IP}:${NEXUS_PORT}",
                    groupId : 'org.springframework.samples',
                    environment : "${evn.BUILD_ID}- ${evn.BUILD_TIMESTAMP}",
                    repository : "${RELEASE_REPO}",
                    
                )
            }
        }
     }
}
