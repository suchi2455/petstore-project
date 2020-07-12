node("master"){
stage('Preparation') {
    git branch: "${env.BRANCH_NAME}",
                url: 'https://github.com/suchi2455/petstore-project.git',
                credentialsId: 'gitHubCredential'
            
        }
        stage('Deploy to Dev') {
            environment{
                RETRY = '80'
            }
            
                echo 'Logging into $DEV_ENV'
                withCredentials([usernamePassword(credentialsId: 'apim_dev', usernameVariable: 'DEV_USERNAME', passwordVariable: 'DEV_PASSWORD')]) {
                    sh 'apictl login $DEV_ENV -u $DEV_USERNAME -p $DEV_PASSWORD -k'                        
                }
                echo 'Deploying to $DEV_ENV'
                sh 'apictl import-api -f $API_DIR -e $DEV_ENV -k --preserve-provider --update --verbose'
         
        }
        stage('Run Tests') {
        
                echo 'Running tests in $DEV_ENV'
          
        }
        stage('Deploy to Production') {
            environment{
                RETRY = '60'
            }
        
                sh 'echo "Logging into $PROD_ENV"'
                withCredentials([usernamePassword(credentialsId: 'apim_prod', usernameVariable: 'PROD_USERNAME', passwordVariable: 'PROD_PASSWORD')]) {
                    sh 'apictl login $PROD_ENV -u $PROD_USERNAME -p $PROD_PASSWORD -k'                        
                }
                echo 'Deploying to Production'
                sh 'apictl import-api -f $API_DIR -e $PROD_ENV -k --preserve-provider --update --verbose'
        }

}
