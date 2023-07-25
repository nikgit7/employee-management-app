pipeline {
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhublogin')
    image1 = "nikhildocker28/frontend-k8s"
    image2 = "nikhildocker28/k8s-backend"
    image3 = "nikhildocker28/k8s-db"
  }
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        git 'https://github.com/nikgit7/employee-management-app.git'
      }
    }
    
    stage('Build Image') {
      steps {
        script {
          sh "docker build -t $image1 frontend/ "
          sh "docker build -t $image2 backend/ "
          sh "docker build -t $image3 database/ "
        }
      }
    }
    stage('Login DockerHub'){
      steps{
        sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
      }
    }
    stage ('Pushing Image to DockerHub'){
        script {
            sh "docker push nikhildocker28/frontend-k8s:latest"
            sh "docker push nikhildocker28/k8s-backend:latest"
            sh "docker push nikhildocker28/k8s-db:latest"
          }
        }
      }
    }
    stage('Deploy to Minikube') {

      steps {
        script {

              sh "kubectl apply -f frontend/manifests/frontenddeployandsvc.yaml && kubectl apply -f backend/manifests/backenddeployandsvc.yaml && kubectl apply -f database/manifests/mysqldeploymentandsvc.yaml"

         }
      }
    }
