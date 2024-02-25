pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "omar9289/nightwolf"
    }

    stages {
      stage('Clone repository') {  
        steps {
        checkout scm
    }
      }


     stage('Test and Build Docker Image') {
         steps {       
         script {
                    env.GIT_COMMIT_REV = sh (script: 'git log -n 1 --pretty=format:"%h"', returnStdout: true)
                    // env.GIT_COMMIT_REV = sh (script: 'git rev-parse HEAD | cut -c -7', returnStdout: true)
                    customImage = docker.build("${DOCKER_IMAGE_NAME}:${GIT_COMMIT_REV}")
                    }
                    }                           
     }                               
     stage('Push Docker Image') {
      steps {  
            
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        customImage.push("${GIT_COMMIT_REV}")
                    
                    }
                }
            }
   }

        // stage('Trigger CD job ') {
        //         steps {
        //         echo "triggering CD"
        //         build job: 'CD', parameters: [string(name: 'GIT_COMMIT_REV', value: env.GIT_COMMIT_REV)]
        // }
        // }
        
  }
 }