properties([parameters([string(defaultValue: 'kube@192.168.76.139', description: '', name: 'ServerDeployment', trim: false), choice(choices: ['YES', 'NO'], description: '', name: 'BUILD'), string(defaultValue: 'main', description: '', name: 'Branch', trim: false)])])

pipeline 
{
    agent any
    environment {
        BIN = tool 'BIN',type: 'dockerTool'
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
            
            sh "$BIN/docker build -t docker89781/image:${BUILD_NUMBER} ."
        
            sh "$BIN/docker tag docker89781/image:${BUILD_NUMBER} docker89781/image:latest"
            sh "$BIN/docker login --username=docker89781 --password=Devops@8978"
            sh "$BIN/docker push docker89781/image:latest"
            
            
            }
        }
     stage('deploy to k8 env') 
       {
           steps
           {

             
             sh "$BIN/kubectl apply -f ."
             
           }
       }
    }
    
    
}
