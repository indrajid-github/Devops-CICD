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
    stages
    {
        stage('SCM checkout')
        {
            steps
            {
                checkout scmGit(branches: [[name: '*/feature']], 
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
                    //withCredentials([string(credentialsId: 'sonar-token', variable: 'sonar-token')]) 
                    //{
                        sh 'mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=jenkins-intergration-key \
                            -Dsonar.host.url=http://13.236.177.156:9000 \
                            -Dsonar.login=squ_5cd2b03357fb9c74a8b3e030b95489958863f84c'
                    //}               
            }
        }

    }
}