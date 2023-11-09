pipeline
{
    agent //any
    {
        //label 'maven'
        node
        {
            label 'maven'
            customWorkspace '/home/jenkins/workspace/devops-cicd'
        }
    }
    tools
    {
        maven 'mvn-3.9.5'
    }
    parameters
    {
        string(name: 'branch', defaultValue: 'master', description: 'Enter branch name')
    }
    stages
    {
        stage('SCM checkout')
        {
            steps
            {
                checkout scmGit(branches: [[name: '*/${params.branch}']], 
                extensions: [], userRemoteConfigs: [[credentialsId: 'git-cred', url: 'https://github.com/indrajid-github/Devops-CICD.git']])
            }
        }
        stage('Code Compile')
        {
            steps
            {
                sh 'mvn clean compile'
            }
        }
        stage('Code Analysis')
        {
            steps
            {    
                            withCredentials([string(credentialsId: 'sonar-token', variable: 'sonar_token')]) 
                            {
                                sh 'mvn sonar:sonar \
                                    -Dsonar.projectKey=jenkins-intergration-key \
                                    -Dsonar.host.url=http://3.25.181.53:9000 \
                                    -Dsonar.login=$sonar_token'
                            }   
                }   
        }

    }
}