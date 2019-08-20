node{
  def Namespace = "default"
  def ImageName = "takoua113/cicd"
  def Creds	= "takoua_docker_hub"
  def NODE_PORT = 31000 + "${ghprbPullId}".toInteger()
  try{
    stage('Checkout'){
      git(url: "https://github.com/takoua-kharroubi/ci-cd.git", branch: "${ghprbSourceBranch}")
      imageTag = NODE_PORT
    } 
    stage('RUN Unit Tests'){
      sh "npm install"
      sh "npm test"
    }
    stage('Docker Build') {
      sh "docker build -t ${ImageName}:${imageTag} ."
    }
    
    stage('Docker Build, Push'){
      withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {        
        sh "docker push ${ImageName}"
      }
    }    
    
  } catch (err) {
    currentBuild.result = 'FAILURE'
  }
}
