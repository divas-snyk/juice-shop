pipeline {
    agent any

    tools { nodejs "NodeJS" }
    stages {
stage('Install Snyk CLI and Snyk Filter') {
            steps {
                script {
			sh 'npm install -g snyk'
		    	sh 'npm i -g snyk-filter'
			//sh 'sudo apt-get install -y jq'
			
                }
            }
        }
stage('Authorize Snyk CLI') {
            steps {
                    sh 'snyk auth 7865b608-194c-45c5-9310-bf88b7f6aaa2'
            }
        }
stage('Build Image') {
      		steps {
        		sh 'docker build -t divassnyk/juice-shop --file=Dockerfile'
      		}
    	}
stage('Snyk Container Test') {
      		steps {
			catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        			sh 'snyk container test juice-shop --file=Dockerfile --severity-threshold=critical --exclude-base-image-vulns --sarif-file-output=results-container.sarif'
			}
			recordIssues tool: sarif(name: 'Snyk Container', id: 'snyk-container', pattern: 'results-container.sarif')
      		}
    	}
}
}
