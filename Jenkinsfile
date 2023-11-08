pipeline
{
    agent //any
    {
        //label 'maven'
        node
        {
            label 'maven'
            customWorkspace '/home/indrajid/workspace/devops-cicd'
        }
    }
    stages
    {
        stage('SCM checkout')
        {
            steps
            {
                sh 'checkout scmGit(branches: [[name: '*/feature']], 
                extensions: [], userRemoteConfigs: [[credentialsId: 'git-cred', url: 'https://github.com/indrajid-github/Devops-CICD.git']])'
            }
        }
    }
}