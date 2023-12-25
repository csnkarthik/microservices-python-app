@Library('devops-projects-shared-lib') _

pipeline {

    agent {
        label 'docker'
    }

    parameters {
        choice choices: ['no', 'yes'], description: 'Are you sure, you want to move?', name: 'action'       
    }

    // environment {
    //     DOCKER_CREDENTIALS = credentials('docker_credentials')
    // }

    stages{
        
        stage('git checkout'){            
                when { expression { params.action == 'create' } }
                steps {                
                    gitCheckout(
                        branch: 'main',
                        url: 'https://github.com/csnkarthik/microservices-python-app.git'
                    )
                }
        }

        stage('move helm configs to the cluster'){
            when { expression { params.action == 'yes' } }            
            steps {         
                input message: 'Are you sure you wanna proceed to deploy?', ok: 'Yes' 
                sshagent(['minikube_cluster']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no gayathrik@192.168.0.104 
                        ssh -o StrictHostKeyChecking=no gayathrik@192.168.0.104 mkdir -p ~/Desktop/installs/microservices-python-app/Helm_charts
                        scp -r ${WORKSPACE}/Helm_charts gayathrik@192.168.0.104:~/Desktop/installs/microservices-python-app/Helm_charts
                    """
                }
            }
        }
       
    }    
}