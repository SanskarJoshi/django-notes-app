pipeline
{
    agent any
    stages
    {
       stage("Build")
        {
            steps
            {
                sh '''
                  tagName=`echo $BRANCH_NAME | sed s#/#-#g`
                  docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/note-app-:${tagName}${BUILD_NUMBER} -f Dockerfile .
                '''
                echo "Building the image..."

            }
        }
        stage("Push to AWS ECR")
        {
            steps
            {
               sh '''
                    tagName=`echo $BRANCH_NAME | sed s#/#-#g`
                    docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/note-app:${tagName}${BUILD_NUMBER}
                '''         
                echo "Pushing the image to ECR..."
            }
        }
        stage("Delete Local Images") {
            steps {
                sh '''
                    tagName=`echo $BRANCH_NAME | sed s#/#-#g`
                    docker rmi ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/note-app:${tagName}${BUILD_NUMBER}
                '''
                echo "Local images deleted after pushing to ECR."
            }
        }
       
        stage("k8s")
        {
            steps
            {
               withKubeConfig([credentialsId: 'jenkins-k8s-integration', serverUrl: 'https://C7E621965D521A1163A51E20C09DCA17.gr7.us-east-1.eks.amazonaws.com']) {
                   sh '''
                    pwd
                   ''' 
               }
            }
        }
    }
}
