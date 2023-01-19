// pipeline {
//     agent any
//     environment {
//         AWS_ACCOUNT_ID="5878-8080-2577"
//         AWS_DEFAULT_REGION="us-east-1" 
//         CLUSTER_NAME="jenkins-cluster"
//         SERVICE_NAME="ecs-jenkins-pipeline-service"
//         TASK_DEFINITION_NAME="first-run-task-definition"
//         DESIRED_COUNT="1"
//         IMAGE_REPO_NAME="ecr"
//         IMAGE_TAG="${env.BUILD_ID}"
//         REPOSITORY_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
//         registryCredential="Demo-Jenkins-Pipeline"
//     }
   
//     stages {

//     // Tests
//     stage('Unit Tests') {
//       steps{
//         script {
//           sh 'npm install'
// 	  sh 'npm test -- --watchAll=false'
//         }
//       }
//     }
        
//     // Building Docker images
//     stage('Building image') {
//       steps{
    
//         script {
//           dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
//         }
//       }
//     }
   
//     // Uploading Docker images into AWS ECR
//     stage('Pushing to ECR') {
//      steps{  
//          script {
//            docker.withRegistry("https://" + REPOSITORY_URI, "ecr:${AWS_DEFAULT_REGION}:" + registryCredential) {
//        dockerImage.push()
//            }
//          }
//         }
//       }
      
//     stage('Deploy') {
//      steps{
//             withAWS(credentials: registryCredential, region: "${AWS_DEFAULT_REGION}") {
//                 script {
// 			sh './script.sh'
//                 }
//             } 
//         }
//       }      
      
//     }
// }
pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="5878-8080-2577"
        AWS_DEFAULT_REGION="us-east-1" 
        CLUSTER_NAME="jenkins-cluster"
        SERVICE_NAME="ecs-jenkins-pipeline-service"
        TASK_DEFINITION_NAME="first-run-task-definition"
        DESIRED_COUNT="1"
        IMAGE_REPO_NAME="ecr"
        IMAGE_TAG="${env.BUILD_ID}"
        REPOSITORY_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
        registryCredential="Demo-Jenkins-Pipeline"
    }
  stages {
    stage('Cloning Git') {
      steps {
                checkout scm

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build IMAGE_REPO_NAME
        }
      }
    }
   
stage('push Image') {

      steps{
        script {
          docker.withRegistry(REPOSITORY_URI, 'ecr:us-east-1:registryCredential') {     
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }

 
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $IMAGE_REPO_NAME:$BUILD_NUMBER"
         sh "docker rmi $IMAGE_REPO_NAME:latest"

          }// End of remove unused docker image for master
      }  
    } //end of pipeline
}
