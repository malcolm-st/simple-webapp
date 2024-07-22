pipeline {
    agent any
    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    git branch: "master",
                        url: "https://github.com/malcolm-st/simple-webapp.git"
                }
            }
        }
        stage('OWASP DependencyCheck') {
           	steps {
				 dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint 
					--noupdate
					--suppression suppression.xml''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
			}
        }
    }   
    post {
        success {
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }
    }
}