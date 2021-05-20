pipeline {
	agent any
	stages {
		stage("Checkout code") {
			steps {
				checkout scm
			}
		}


	// Develop chain

		stage('Build image with tag latest for develop branch') {
			when{
				expression{env.GIT_BRANCH == 'origin/develop'}
			}
			steps{
				script {
					ms = docker.build("${gcp_project_name}/${microservice_name}","-f ./cicd/Dockerfile ./ ")
				}
			}
		}

		stage('Push image with tag latest for develop branch') {
			when{
				expression{env.GIT_BRANCH == 'origin/develop'}
			}
			steps{
				script {
					docker.withRegistry("${gcr_url}", "gcr:${gcr_admin_key}") {
						ms.push("latest")
					}                    
				}
			}
		}

		stage ('Deploy image on dev cluster') {
			when{
				expression{env.GIT_BRANCH == 'origin/develop'}
			}
			steps{
				build job: 'deploy-to-k8s'
			}    
		}
	}
}
