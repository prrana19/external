def ProjectId="dtc-user5"
pipeline{
    agent any
    environment {
        Image_name = "gcr.io/${ProjectId}/external-image:V.${BUILD_ID}"
    }
    stages{
        stage('dependancy versions'){
            steps{
                sh '''
                    git --version
                    docker --version
                    npm -v
                '''
            }    
        }
        stage('git checkout'){
            steps{
                git 'https://github.com/dnizam/bootcamp-external.git'
            }    
        }
        stage('git test'){
            steps{
                sh '''
                    ls -a
                    echo "install dependencies and test external code ..!"
                    npm install
                    #npm test 
                ''' 
            }    
        }
        stage('build'){
            steps{
                sh '''
                    echo "${Image_name}"
                    echo "build and push docker image for external app ..!"
                    gcloud builds submit --tag ${Image_name} .
                ''' 
            }    
        }
        stage('deploy'){
            steps{
                sh """
                    gcloud container clusters get-credentials user5-kube-cluster --zone us-central1-c --project dtc-user5
                    kubectl set image deployment/events-web events-web=${Image_name}
                """ 
            }    
        }  
    }
}
