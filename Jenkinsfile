// prerequisites: a nodejs app must be deployed inside a kubernetes cluster
// TODO: look for all instances of [] and replace all instances of 
//       the 'variables' with actual values 
// variables:
//      [GITREPO]
//      [PROJECTID]
//      [CLUSTER_NAME] 
//      [ZONE]
//      the following values can be found in the yaml:
//      [DEPLOYMENT_NAME]
//      [CONTAINER_NAME] (in the template/spec section of the deployment)

pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Retrieving source from github' 
                git branch: 'master',
                    url: '[GITREPO]'
                //git '[GITREPO]'
                echo 'Did we get the source?' 
                sh 'ls -a'
            }
        }
         
         stage('Stage 2') {
            steps {
                echo 'install dependencies' 
                sh 'npm install'
            }
        }        
         stage('Stage 3') {
            steps {
                echo 'Build Docker Image in GCR'
                sh "gcloud builds submit -t gcr.io/[PROJECTID]/internal:v2.${env.BUILD_ID} ."
            }
        }        
         stage('Stage 4') {
            steps {
                echo 'Get cluster credentials'
                sh 'gcloud container clusters get-credentials [CLUSTER_NAME] --zone [ZONE] --project [PROJECTID]'
                echo 'Update the image'
                sh "kubectl set image deployment/[DEPLOYMENT_NAME] [CONTAINER_NAME]=gcr.io/[PROJECTID]/internal:v2.${env.BUILD_ID} --record"
            }
        }        
               

        
    }
}


pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Retrieving source from github' 
                git branch: 'master',
                    url: 'https://github.com/imGaurav3/myExternal.git'
                //git '[GITREPO]'
                echo 'Did we get the source?' 
                sh 'ls -a'
            }
        }
         
         stage('Stage 2') {
            steps {
                echo 'install dependencies' 
                sh 'npm install'
            }
        }        
         stage('Stage 3') {
            steps {
                echo 'Build Docker Image in Docker'
                sh "gcloud builds submit -t gcr.io/may2021dtc102/external:v2.${env.BUILD_ID} ."
                
            }
        }        
         stage('Stage 4') {
            steps {
                echo 'Get cluster credentials'
                sh 'gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project may2021dtc102'
                echo 'Update the image'
                sh "kubectl set image deployment/deployment-external external-container=gcr.io/may2021dtc102/external:v2.${env.BUILD_ID} --record"
            }
        }        
               

        
    }
}


pipeline {
    agent any 
    stages {
        stage('Stage 1: Git Cloning') {
            steps {
                echo 'Retrieving source from github' 
                git branch: 'master',
                    url: 'https://github.com/imGaurav3/eventExternal.git'
                //git '[GITREPO]'
                echo 'Did we get the source?' 
                sh 'ls -a'
            }
        }
         
         stage('Stage 2: Installing dependencies') {
            steps {
                echo 'install dependencies' 
                sh 'npm install'
            }
        }       
         stage('Stage 3: Automated testing') {
            steps {
                echo 'Mocha automated testing' 
                sh 'npm test'
            }
        }
         stage('Stage 4: Building new image') {
            steps {
                echo 'Build Docker Image in GCR'
                sh "gcloud builds submit -t gcr.io/meta-buckeye-314515/external:v1.${env.BUILD_ID} ."
                //sh "docker build -t imgaurav3/external:v2.${env.BUILD_ID} ."
            }
        }        
         stage('Stage 5: Updating image name in K8 running deployment') {
            steps {
                echo 'Get cluster credentials'
                sh 'gcloud container clusters get-credentials my-first-cluster-1 --zone us-central1-c --project meta-buckeye-314515'
                echo 'Update the image'
                sh "kubectl set image deployment/external-deployment external-container=gcr.io/meta-buckeye-314515/external:v1.${env.BUILD_ID}"
            }
        }        
               

        
    }
}