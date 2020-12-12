node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.3", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t bairav004/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u bairav004 -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push bairav004/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app bairav004/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.95.218 docker stop java-web-app || true'
          sh 'ssh  ubuntu@65.0.95.218 docker rm java-web-app || true'
          sh 'ssh  ubuntu@65.0.95.218 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@65.0.95.218 ${dockerRun}"
       }
       
    }
     
     
}
