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
    environment
    {
        build_num = getBuildNumber()
    }
    stages
    {
        stage('SCM checkout')
        {
            steps
            {
                checkout scmGit(branches: [[name: "${params.branch}"]],
                userRemoteConfigs: [
                    [ url: 'https://github.com/indrajid-github/Devops-CICD.git' ]
                ])
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
                                    -Dsonar.projectKey=Devops-CICD \
                                    -Dsonar.host.url=http://3.25.181.53:9000 \
                                    -Dsonar.login=$sonar_token'
                            }   
            }   
        }
        stage('Code Build')
        {
            steps
            {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build')
        {
            steps
            {
                sh "docker build . -t uriyapraba/devopscicd:${build_num}"
            }
        }
        stage('Docker Push')
        {
            steps
            {
                script
                {
                    withCredentials([string(credentialsId: 'dockerHub-cred', variable: 'dockerHub-cred')]) 
                    {
                        sh 'docker login -u uriyapraba -p $dockerHub-cred'
                        docker.image.push("uriyapraba/devopscicd:${build_num}")
                    }
                }
                
            }
        }
    }
}
def getBuildNumber()
{
    def buildNumber = currentBuild.number
    return buildNumber
}