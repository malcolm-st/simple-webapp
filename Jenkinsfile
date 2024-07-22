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
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP2 -Dsonar.host.url=http://172.19.0.4:9000 -Dsonar.sources=. -Dsonar.java.binaries=target/classes"
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