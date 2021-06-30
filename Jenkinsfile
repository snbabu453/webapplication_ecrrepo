pipeline {
   agent any
   environment 
    {
        VERSION = "${BUILD_NUMBER}"
        PROJECT = 'nodeapp30'
        IMAGE = "$PROJECT:$VERSION"
        ECRURL = 'https://291427156641.dkr.ecr.us-east-1.amazonaws.com/nodeapp30'
       // awscredentails : Id of aws credentails in jenkins manage user credentails
        ECRCRED = 'ecr:us-east-1:awscredentails'
    }   
    stages {
      stage('GetSCM') {

         steps {

            // Get some code from a GitHub repository

            git 'https://github.com/snbabu453/webapplication_ecrrepo.git'
         }
         }
         stage('Image Build'){
             steps{
                 script{
                       docker.build('$IMAGE')
                 }
             }
         }
         stage('Push Image'){
         steps{
             script
                {
                    docker.withRegistry(ECRURL, ECRCRED)
                    {
                        docker.image(IMAGE).push()
                    }
                }
            }
         }
    }
    post
    {
        always
        {
            // make sure that the Docker image is removed

            sh "docker rmi $IMAGE | true"
        }
    }
}
