pipeline {
  environment {
    registry = "shreejaa/cloudserver"
    registryCredential = 'dockerhub'
	giturl="https://ShreeTalla@bitbucket.org/ShreeTalla/cloud-server.git"
  }
  agent any
  stages {
	stage('Clean workspace') {
      steps{
        script {
          cleanWs()
        }
      }
    }
    stage('Code Checkout from Git') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
		 doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
		 userRemoteConfigs: [[url: giturl]]])
      }
    }
	stage('Clean stage') {
		  steps{
			script {
			  bat "mvn clean"
			}
		  }
		}
	stage('Compile stage') {
      steps{
        script {
          bat "mvn compile"
        }
      }
    }
	stage('Test stage') {
      steps{
        script {
          bat "mvn test"
        }
      }
    }
	stage('Building Artifact stage') {
      steps{
        script {
          bat "mvn install -DskipTests"
        }
      }
    }
    stage('switching to docker') {
      steps{
        script{
          bat "cd C:\\Program Files\\Docker Toolbox docker.exe -SwitchDaemon"
        }
      }
    }
    stage('Building image stage') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image stage') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}