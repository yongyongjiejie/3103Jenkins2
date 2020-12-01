pipeline {
	agent {
		docker {
			image 'composer:latest'
		}
	}
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
		stage('Install') {
			steps {
				sh 'composer install'
			}
		}
		stage('PHPunit') {
			steps {
                		sh"./vendor/bin/phpunit tests --log-junit logs/unitreport.xml -c tests/phpunit.xml tests"
            		}
		}
		stage('Code Quality Check via SonarQube') {
			steps {
				script {
					def scannerHome = tool 'SonarQube';
					withSonarQubeEnv() {
						sh "${tool("SonarQube")}/bin/sonar-scanner\-Dsonar.projectKey=yongyongjiejie\-Dsonar.sources=.\-Dsonar.host.url=http://192.168.174.128:9000\-Dsonar.login=3fefcae3f5dcd8716e216057f5cfee23c1018476"
						}
					}
				}
		}

	}	
	post {
        	success {
            		dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        	}
		always{
            		junit testResults: 'logs/unitreport.xml'
        	}
    	}
}
