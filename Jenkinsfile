pipeline
{
    agent
    {
        label 'maven'
    }
    environment
    {
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }
    stages
    {
        stage("Cleanup workspace")
        {
            steps
            {
                cleanWs()
            }
        }
        stage("Checkout SCM")
        {
            steps
            {
                script
                {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], 
                    userRemoteConfigs: [[url: 'https://github.com/indrajid-github/complete-production-e2e-deployment.git']])
                }
            }
        }
        stage("update the deployment tag")
        {
            steps
            {
                sh """
                    cat deployment.yaml
                    sed -i '/s${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml 
                    cat deployment.yaml
                """
            }
        }
        stage("push the updated image tag")
        {
            steps
            {
                script
                {
                    sh """
                        git config --global user.name "Indrajith"
                        git config --global user.mail "mailtoindrajith@gmail.com"
                        git add .
                        git commit -m "${IMAGE_TAG} updated"
                    """
                    withDockerRegistry(credentialsId: 'github_tocken') 
                    {
                        git push https://github.com/indrajid-github/complete-production-e2e-deployment.git main
                    }
                }
            }
        }
    }
}
