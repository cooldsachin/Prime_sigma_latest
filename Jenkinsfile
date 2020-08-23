pipeline {
    agent any
    environment {
        PROJECT_ID = 'parabolic-rope-278721'
        CLUSTER_NAME = 'my-cluster'
        LOCATION = 'us-central1-c'
        CREDENTIALS_ID = 'parabolic-rope-278721'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("cooldsachin/react-ui:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to GKE Cluster') {
            steps{
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'app-deployment.yml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                //step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubectl apply -f app-deployment.yml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                //step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubectl rollout restart deployment.apps/voting-app-deployment', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}
