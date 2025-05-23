def registry= 'https://trialkut2oe.jfrog.io'

pipeline{
	agent any
	environment{
		PATH="/opt/maven/bin:$PATH"
	}
	stages{
		stage('build'){
			steps{
				echo '--buid started--'
				sh 'mvn  clean deploy -Dmaven.test.skip=true'
				echo '--build stoped--'
			}
		}
		stage('test'){
			steps{
				echo '--unit test started--'
				sh 'mvn surefire-report:report'
				echo '--testing is completed--'
			}
		}
		
		stage('Sonar Qube Analysis Stage'){
			environment{
				scannerHome= tool 'sai-sonar-scanner'
			}
			steps{
				withSonarQubeEnv('sai-sonarqube-server'){
					sh "${scannerHome}/bin/sonar-scanner"
				}
			}
		}
		stage("Jar Publish") {                // 14  // Creates a stage named 'Jar Publish'
            steps {                           // 15  // Defines the steps that will be executed in this stage
                script {                      // 16  // Allows running custom Groovy script inside the pipeline
                    echo '<--------------- Jar Publish Started --------------->'  
                                              // Logs a message indicating the start of JAR publishing
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "jfrog-credential"  
                                              // Defines the Artifactory server with the specified URL and credentials
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"  
                                              // Sets properties like build ID and Git commit ID for the build
                    def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "kris-libs-release-local/{1}",
                              "flat": "false",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""  
                                              // Defines the upload specification for uploading JAR files to Artifactory
                    def buildInfo = server.upload(uploadSpec)  
                                              // Uploads the files to Artifactory and collects build info
                    buildInfo.env.collect()  
                                              // Collects environment variables as part of the build info
                    server.publishBuildInfo(buildInfo)  
                                              // Publishes the build information to Artifactory
                    echo '<--------------- Jar Publish Ended --------------->'  
                                              // Logs a message indicating the end of JAR publishing
                }                             // 16  // Ends the script block for 'Jar Publish' stage
            }                                 // 15  // Ends the steps block for 'Jar Publish' stage
        }                                     // 14  // Ends the 'Jar Publish' stage

	}
}
