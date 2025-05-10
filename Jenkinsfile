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
		
	}
}
