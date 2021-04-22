pipeline {
    agent { 
        kubernetes{
            label 'jenkins-slave'
        }
        
    }
    environment{
        registry = "elontsi007/python-app1"
        registryCredential = 'Dockerhub'
        dockerImage = ''
    }
    stages {

        stage('CheckOut') {
            steps{
                sh "pwd"
                sh "ls -l"
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/elontsi/Kubernetes-Deployments.git']]])
            }
        }
        
        stage('Move to Build Directory') {
            steps {
                dir('/home/jenkins/agent/workspace/deploy-to-kubernetes/'){
                    sh "ls -l"
                    script {
                        sh "docker build -t elontsi007/python-app ."
                    }
                }
                sh "ls -l"
            }
        }
        
        stage('Image upload') {
            steps {
                sh "docker images"
                sh "docker login -u 'elontsi007' -p 'Emma1992'"
                sh "docker push elontsi007/python-app"
            }
        }
        
        stage('Deploy App') {
            steps {
                script {
                    kubernetesDeploy(configs: "app-deployment.yaml", kubeconfigId: "kubeconfig")
                }
            }
        }
        
    }
}

