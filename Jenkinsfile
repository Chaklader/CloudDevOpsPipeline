pipeline {
	
    agent any
	
    stages {
	    
        stage('Step #1: Lint HTML') {
            steps {
		sh 'echo "Lint check..."'
                sh 'tidy -q -e *.html'
            }
        }

        stage('Step #2: Lint Dockerfile') {
            steps {
                sh '''
                    if [ -f hadolint_lint.txt ]; then
                        rm hadolint_lint.txt
                    fi
                '''    
                sh 'hadolint ./Dockerfile | tee -a hadolint_lint.txt'
                sh '''
                        lintErrors=$(stat --printf="%s"  hadolint_lint.txt)

                        if [ "$lintErrors" -gt "0" ]; then
                            echo "Errors have been found, please see below"
                            cat hadolint_lint.txt
                            exit 1
                        else
                            echo "There are no erros found on Dockerfile!!"
                        fi
                '''
            }
        }
	    
	stage('Build Docker Image') {
   	    steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
		    
                    sh 'echo "Building Docker Image..."'
     	    	    sh 'docker build -t chaklader/capstone-project-image .'
		}
            }
        }
	    
	stage('Push Image To Dockerhub') {
   	    steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
		    sh 'echo "Pushing Docker Image..."'

     	    	    sh '''
                        docker login -u $USERNAME -p $PASSWORD
			docker push chaklader/capstone-project-image
                    '''
		}
            }
        }

	stage('Deploying') {
              steps{
                  echo 'Deploying to AWS...'
                  withAWS(credentials: 'aws-static', region: 'us-east-1') {

                      sh "aws eks --region us-east-1 update-kubeconfig --name capstoneproject"
                      sh "kubectl config use-context arn:aws:eks:us-east-1:602502938985:cluster/capstoneproject"

                //       sh "kubectl apply -f capstone-k8s.yaml"

                      sh "kubectl get nodes"
                      sh "kubectl get deployments"
                      sh "kubectl get pod -o wide"
                      sh "kubectl get service/capstoneproject"
                  }
              }
        }

        stage('Checking if app is up') {
              steps{
                  echo 'Checking if app is up...'
                  withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1') {

                     sh "curl a238875e75a6a4d6faa5c4a8773bccb8-312821804.us-east-1.elb.amazonaws.com:9080"
                    //  slackSend(message: "The app is up at: ad0e6a88870a9477989eb79393197b59-2120449898.ap-south-1.elb.amazonaws.com:9080", sendAsText: true)
                  }
               }
        }

        
        stage('Checking rollout') {
              steps{
                  echo 'Checking rollout...'
                  withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1') {
                     sh "kubectl rollout status deployments/capstone-app-sagarnil"
                  }
              }
        }
        stage("Cleaning up") {
              steps{
                    echo 'Cleaning up...'
                    sh "docker system prune"
              }
        }
	 
	    
	// stage('Configure kubectl') {
	//     steps {
	// 	withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-west-1a') {
	// 	    sh 'echo "Configure kubectl..."'
	// 	    sh '/usr/local/bin/aws eks --region us-west-1 update-kubeconfig --name capstoneappprojectffff' 
	// 	}
	//     }
        // }

	// stage('Deploy blue container') {
	//     steps {
	// 	withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-west-1a') {
	// 	    sh 'echo "Deploy blue container..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./blue/blue.yaml'
	// 	}
	//     }
	// }
	    	    
	// stage('Deploy green container') {
	//     steps {
	// 	withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-west-1a') {
	// 	    sh 'echo "Deploy green container..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./green/green.yaml'
	// 	}
	//     }
	// }
	    
	// stage('Create blue service') {
	//     steps {
	// 	withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-west-1a') {
	// 	    sh 'echo "Create blue service..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./blue/blue_service.yaml'
	// 	}
	//     }
	// }
	    
	// stage('Update service to green') {
	//     steps {
	// 	withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-west-1a') {
	// 	    sh 'echo "Update service to green..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./green/green_service.yaml'
	// 	}
	//     }
	// }	
   }   
	    
}
