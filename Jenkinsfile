pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/prakashk0301/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("pkw0301/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      //stage("Push image") {
      //      steps {
      //          script {
      //              docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
      //                      myapp.push("latest")
      //                      myapp.push("${env.BUILD_ID}")
      //              }
      //          }
      //      }
      //  }

    
    stage ('deploy k8s ssh agent method') {
      steps {
       sshagent(['k8s-master']) {
        sh "scp -o StrictHostKeyChecking=no hellowhale.yml ubuntu@172.31.22.213:/home/ubuntu/"
         script {
           try {
             sh "ssh ubuntu@172.31.22.213 kubectl apply -f ."
                        }
           catch(error){
           sh "ssh ubuntu@172.31.22.213 kubectl create -f ."}
         }
}
      } 
    }
    
   // stage('Deploy App') {
   //   steps {
   //     script {
   //       kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "KUBERNETES_CLUSTER_CONFIG_MAY")
   //     }
   //   }
   // }

  }

}
