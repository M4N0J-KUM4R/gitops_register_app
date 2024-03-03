pipeline {
  agent {label "Jenkins_Agent"}
  environment {
            APP_NAME = "register_app_pipeline"
  }
  stages {
    stage("CleanUp WorkSpace") {
       steps{
         cleanWs()
       }  
    }
    stage("Checkout From SCM") {
       steps{
         git branch: 'main', credentialsId: 'Github', url: 'https://github.com/M4N0J-KUM4R/gitops_register_app'
       }
    }
    stage("Update the Deployment Tags"){
       steps{
         sh """
            cat deployment.yaml
            sed -i 's/${APP_NAME}.*/${APP_NAME}:$/{IMAGE_TAG}/g' deployment.yaml
            cat deployment.yaml
          """
       }
    }
    stage("Update the Deployment Tags"){
       steps{
         sh """
            git config --global user.name "M4N0J-KUM4R"
            git config --global user.email "androhavoc@gmail.com"
            git add deployment.yaml
            git commit -m "Updated Deployment Manifest"
          """
          withCredentials([gitUsernamePassword(credentialsId: 'Github', gitToolName: 'Default')]){
            sh "git push https://github.com/M4N0J-KUM4R/gitops_register_app main"
          }
       }
    }
  }
}
