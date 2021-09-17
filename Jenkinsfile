properties([parameters([string(defaultValue: 'kube@192.168.76.139', description: '', name: 'ServerDeployment', trim: false), choice(choices: ['YES', 'NO'], description: '', name: 'BUILD'), string(defaultValue: 'main', description: '', name: 'Branch', trim: false)])])

pipeline 
{
    agent any
    environment {
        Docker = "/usr/local/bin/docker"
        kubectl = "/usr/local/bin/kubectl"
        ScannerHome= "/usr/local/Cellar/sonar-scanner"
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
            
        stage('sonar'){ 
            steps{
                
                    sh "${ScannerHome}/4.6.2.2472_1/libexec/bin -Dproject.settings=sonar-project.properties"
                } 
            }
        }
        
    
        stage('docker Build')
        {
            when {
             expression { params.BUILD == 'YES' }
                }
            steps 
            {
            
                sh "${Docker} build -t docker89781/image:${BUILD_NUMBER} ."
        
                sh "${Docker} tag docker89781/image:${BUILD_NUMBER} docker89781/image:latest"
                sh "${Docker} login --username=docker89781 --password=Devops@8978"
                sh "${Docker} push docker89781/image:latest"
            
            
            }
        }
     stage('deploy to k8 env') 
       {
           when {
             expression { params.BUILD == 'YES' }
           }
           steps
           {

             
               sh "${kubectl} delete -f ."
               sh "${kubectl} create -f ."
             
           }
       }
    }
    
    
}
