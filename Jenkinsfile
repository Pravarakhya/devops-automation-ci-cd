pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Pravarakhya/devops-automation-ci-cd.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t pravarakhya/kubernetes .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'BIGtree@97', variable: 'BIGtree@97')]) {
                    sh 'docker login -u pravarakhya -p BIGtree@97'
                        
                    }
                    sh 'docker push pravarakhya/kubernetes'
                }
            }
        }
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    
    }    
}
