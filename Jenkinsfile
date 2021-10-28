pipeline {
	
    agent any
	
    stages {
	    
        // stage('Step #1: Lint HTML') {
        //     steps {
        //         sh 'echo "Lint check..."'
        //             sh 'tidy -q -e *.html'
        //         }
        // }

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
    
        stage('Step #3: Build Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
            
                    sh 'echo "Building Docker Image..."'
                    sh 'docker build -t chaklader/capstone-project-image .'
                }
            }
        }
            
        stage('Step #4: Push Image To Dockerhub') {
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

        stage('Step #5: Kubernetes cluster configuration setup') {
            steps{
                echo 'Deploying to AWS...'
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1a') {

                    sh "aws eks --region us-east-1 update-kubeconfig --name capstoneproject"
                    sh "kubectl config use-context arn:aws:eks:us-east-1:602502938985:cluster/capstoneproject"
                }
            }
        }
    
        stage('Step #6: Deploy Blue container') {
            steps {
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1a') {

                    sh 'echo "Deploy blue container..."'
                    sh 'kubectl apply -f ./blue/blue.yaml'
                }
            }
        }
	    	    
        stage('Step #7: Deploy Green container') {
            steps {
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1a') {

                    sh 'echo "Deploy green container..."'
                    sh 'kubectl apply -f ./green/green.yaml'
                }
            }
        }
	    
        stage('Step #8: Create blue service') {
            steps {
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1a') {

                    sh 'echo "Create blue service..."'
                    sh 'kubectl apply -f ./blue/blue_service.yaml'
                }
            }
        }
            
        stage('Step #9: Update service to green') {

            steps {
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1a') {

                    sh 'echo "Update service to green..."'
                    sh 'kubectl apply -f ./green/green_service.yaml'
                }
            }
        }	

        stage('Step #10: Kubernates deployment info') {

            steps{
                echo 'Kubernates deployment info.....'
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1a') {

                    // sh "kubectl apply -f capstone-k8s.yaml"

                    sh "kubectl get nodes"
                    sh "kubectl get deployments"
                    sh "kubectl get pod -o wide"

                    sh "kubectl get service/bglb"
                }
            }
        }


        // sleep 60
        // sleep 60s
        // sleep 1m

        stage('Step #11: Checking if app is up') {
   
            steps{
                echo 'Checking if app is up...'
                withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1') {

                    sh '''
                        EKS_ELB_HOSTNAME=$(kubectl get service/bglb -o jsonpath='{.status.loadBalancer.ingress[*].hostname}') \
                        && echo $EKS_ELB_HOSTNAME

                        curl $EKS_ELB_HOSTNAME:8000
                    '''
                    //  slackSend(message: "The app is up at: ad0e6a88870a9477989eb79393197b59-2120449898.ap-south-1.elb.amazonaws.com:9080", sendAsText: true)
                }
            }
        }

        // stage('Step #12: Checking rollout') {
        //     steps{

        //         echo 'Checking rollout...'
        //         withAWS(credentials: 'AWS_CREDENTIAL', region: 'us-east-1') {

        //             sh "kubectl rollout status deployments/capstoneproject"
        //         }
        //     }
        // }

        stage("Step #13: Cleaning up") {

            steps{
                echo 'Cleaning up...'
                sh "docker system prune"
            }
        }
	    
    }
}
