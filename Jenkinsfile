pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git branch: 'master', credentialsId: '52d45cec-ce49-4c8f-b04d-1796da3e19bb', url: 'https://github.com/yongyongjiejie/3103Jenkins2.git'
			}
		}
 		stage('OWASP DependencyCheck') {
            		steps {
                		dependencyCheck additionalArguments: '--format HTML --format XML --suppression suppression.xml', odcInstallation: 'OWASP'
            		}
        	}
		stage('Code Quality Check via SonarQube') {
			steps {
				script {
					def scannerHome = tool 'SonarQube';
					withSonarQubeEnv("SonarQube") {
						sh "${tool("SonarQube")}/bin/sonar-scanner -Dsonar.projectKey=yongyongjiejie -Dsonar.sources=. -Dsonar.host.url=http://192.168.174.128:9000 -Dsonar.login=4cdb8be6df0b96e4cc424a9e12bcd9512723318b"
					}
				}
			}
		}

	}	
	post {
        	success {
            		dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        	}
    	}
}
