pipeline{
  agent any
   environment{
     APP_NAME = "gitops-demo-app"
   } 
  stages{
    stage("Cleanup Workspace"){
            steps{
                script{
                    cleanWs()
                }
            }
        }
    stage("Checkout SCM"){
        steps{
            git branch: 'main', 
            credentialsId: 'github',
            url: 'https://github.com/proffdeep/gitops-demo-config.git'
        }
    }
    stage("updating k8 manifest"){
      steps{
          sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml"
          sh "cat deployment.yaml"
        }
      }
    stage("pushing updated code on github"){
      steps{
          script{
              sh """
              git config --global user.name "deepanshu"
              git config --global user.email "deepch98@gmail.com"
              git add deployment.yaml
              git commit -m 'updated the deployment file'
              """
              withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push origin main"
          }
        }
      }
    }
  }
}
