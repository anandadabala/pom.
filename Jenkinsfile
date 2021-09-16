properties([parameters([string(defaultValue: 'kube@192.168.76.139', description: '', name: 'ServerDeployment', trim: false), choice(choices: ['YES', 'NO'], description: '', name: 'BUILD'), string(defaultValue: 'main', description: '', name: 'Branch', trim: false)])])
pipeline 
{
    agent { label 'ubuntuslave' }
    environment {
        
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
            sh "sudo docker build -t docker89781/image:${BUILD_NUMBER} ."
        
            sh "sudo docker tag docker89781/image:${BUILD_NUMBER} docker89781/image:latest"
            sh "sudo docker login --username=docker89781 --password=Devops@8978"
            sh "sudo docker push docker89781/image:latest"
            }
        }
     stage('deploy to k8 env') 
       {
           steps
           {
             sh  "scp Deployment.yaml ${ServerDeployment}:/opt/dockerk8"
             sh "ssh -T ${ServerDeployment} kubectl apply -f /opt/dockerk8/Deployment.yaml"
           }
       }
    }
    
    
}
