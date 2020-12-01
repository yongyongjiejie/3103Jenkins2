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
