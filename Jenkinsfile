pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = "us-west-1"
    }
     stages {
         stage('Github') {
           steps {
            git branch: 'main', url: 'https://github.com/YogenderJ/code-deploy-1.git'
            
            withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId:     "aws-cred",
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    // AWS Code
                    sh ''' 
                        #! /bin/bash 
                        aws deploy create-deployment \
                            --application-name code-deploy-1 \
                            --deployment-config-name CodeDeployDefault.OneAtATime \
                            --deployment-group-name code-deploy-g1 \
                            --description "My GitHub deployment demo" \
                            --github-location repository="YogenderJ/code-deploy-1",commitId="$(git rev-parse HEAD)"
                     '''
                    }
           }
         }
         
         stage('Deploy') { 
           steps {
             echo 'Deployed to Codedeploy'
            }
        }      
    }
    post { 
        always { 
            echo 'Stage is success'
        }
    }
    
    
}
