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
                checkout scmGit(branches: [[name: 'master']],
                userRemoteConfigs: [
                    [ url: 'https://github.com/indrajid-github/complete-e2e-deployment.git' ]
                ])
            }
        }
        stage("update the deployment tag")
        {
            steps
            {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml 
                    cat deployment.yaml
                """
            }
        }
        stage("push the updated image tag")
        {
            steps
            {
                
                    sh """
                        git config --global user.name "Indrajith"
                        git config --global user.email "mailtoindrajith@gmail.com"
                        git add .
                        git commit -m "new build image tag updated"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github_tocken', gitToolName: 'Default')]) 
                    {
                        
                        sh "git push origin master"
                    }
            }
        }
    }
}
