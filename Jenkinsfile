properties([parameters([string(defaultValue: 'kube@192.168.76.139', description: '', name: 'ServerDeployment', trim: false), choice(choices: ['YES', 'NO'], description: '', name: 'BUILD'), string(defaultValue: 'main', description: '', name: 'Branch', trim: false)])])
pipeline 
{
    agent any
    environment {
        PATH = "/usr/local/bin/docker:$PATH"
        BUILD_NUMBER= "${env.BUILD_NUMBER}"
    }
    
    stages
    {
        stage('git')
        {
            steps{
        git branch: '${Branch}', url: 'https://github.com/devopsclouds/warproject.git' 
          }
        }
            
        
    
        stage('docker Build')
        {
            when {
             expression { params.BUILD == 'YES' }
                }
            steps 
            {
            sh "docker build -t docker89781/image:${BUILD_NUMBER} ."
        
            sh "docker tag docker89781/image:${BUILD_NUMBER} docker89781/image:latest"
            sh "docker login --username=docker89781 --password=Devops@8978"
            sh "docker push docker89781/image:latest"
            }
        }
     stage('deploy to k8 env') 
       {
           steps
           {
             
             sh " kubectl apply -f ."
           }
       }
    }
    
    
}
