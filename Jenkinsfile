pipeline {                                    // 1  // Defines the start of the Jenkins pipeline block

    agent any                                 // Specifies the pipeline can run on any available agent

    environment {                             // 2  // Defines environment variables for the pipeline
        PATH = "/opt/maven/bin:$PATH"         // Adds Maven's path to the system's PATH variable
    }                                         // 2  // Ends the environment block

    stages {                                  // 3  // Defines the stages block where multiple stages are declared

        stage("build") {                      // 4  // Creates a stage named 'build'
            steps {
                  sh 'mvn clean install'      // 5  // Defines the steps that will be executed in this stage
		  sh 'touch newfile'
            }
	}
	stage("Sonar Qube Analysis"){
            environment{
		  scannerHome = tool 'sai-sonar-scanner' //Configure our sonnar qube scanner tool name
	    }
	    steps{
		 withSonarQubeEnv('sai-sonarqube-server'){ // Configuree the sonar qube server name
                   //Executes the SonarQube analysis with the environment
		   sh "${scannerHome}/bin/sonar-scanner"  //this will run the sonarar qube tool 
		 }
            }
         }
     }
}
			
		
