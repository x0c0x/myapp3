pipeline {
    agent {
        docker {
            image 'azagramac/maven'
            args '-v /root/.m2:/root/.m2'
        }
    }
    
    environment {
        // This can be nexus3 or nexus2 server
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "192.168.1.100:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY_RELEASES = "maven-releases"
        NEXUS_REPOSITORY_SNAPSHOTS = "maven-snapshots"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexuspw"
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('SonarQube Analytics') {
            steps {
   //              mvnHome =  tool name: 'apache-maven-3.6.1', type: 'maven'
                 withSonarQubeEnv('sonar7') { 
   //              sh "${mvnHome}/bin/mvn sonar:sonar"
                sh "mvn sonar:sonar"
        //        withSonarQubeEnv('sonar-server') {
          //          sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
                }
            }
        }

// Build repo        

        stage('Nexus Repository') {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                                nexusVersion: NEXUS_VERSION,
                                protocol: NEXUS_PROTOCOL,
                                nexusUrl: NEXUS_URL,
                                groupId: pom.groupId,
                                version: pom.version,
                                repository: NEXUS_REPOSITORY_SNAPSHOTS,
                                credentialsId: NEXUS_CREDENTIAL_ID, 
                                artifacts: [
                                    [artifactId: pom.artifactId, 
                                     classifier: '',
                                     file: artifactPath,
                                     type: pom.packaging],
                                    [artifactId: pom.artifactId,
                                     classifier: '',
                                     file: "pom.xml", 
                                     type: "pom"]]);
                      
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
            
            
        stage('Slack Notification'){
             steps{
                script{
                //slackSend channel: '#jenkins-build', color: 'Good', message: 'Welcome to Jenkins', teamDomain: 'x0c0x', tokenCredentialId: 'slacknotification'
               // slackSend color: 'good', message: 'Build is successfully completed', channel: '#jenkins-build'
              //  slackSend (color: 'good', channel: '#jenkins-build', message: "Completed: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    
	currentBuild = 'Pass'
        message = """
        *Jenkins Build*
        Job name: `${env.JOB_NAME}`
        Build number: `#${env.BUILD_NUMBER}`
        Build status: `${currentBuild.result}`
        Build details: <${env.BUILD_URL}/console|See in web console>
    """.stripIndent()
	slackSend(color: 'good', message: message)
			            }
			        }
		     }                 
        }
    }
