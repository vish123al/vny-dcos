/*def gitCommit() {
    sh "git rev-parse HEAD > GIT_COMMIT"
    def gitCommit = readFile('GIT_COMMIT').trim()
    sh "rm -f GIT_COMMIT"
    return gitCommit
}

node {
    // Checkout source code from Git
    stage 'Checkout'
    checkout scm

    // Build Docker image
    stage 'Build'
    sh "docker build -t vishaldenge/jenkinsfile:${gitCommit()} ."

    // Log in and push image to GitLab
    stage 'Publish'
    withCredentials(
        [[
            $class: 'UsernamePasswordMultiBinding',
            credentialsId: 'vishaldenge',
            passwordVariable: 'PASSWORD',
            usernameVariable: 'USERNAME'
        ]]
    ) {
        sh "docker login -u 'vishaldenge' -p 'v!sh@l123' -e 'vishaldenge1@gmail.com' "
        sh "docker push vishaldenge/jenkinsfile:${gitCommit()}"
    }

    stage 'Deploy'

    marathon(
        url: 'http://10.0.1.85:8080',
        forceUpdate: true,
        filename: 'marathon.json',
        appId: 'vnyuser',
        docker: "vishaldenge/jenkinsfile:${gitCommit()}".toString()
    )
}
*/

def gitCommit() {
    sh "git rev-parse HEAD > GIT_COMMIT"
    def gitCommit = readFile('GIT_COMMIT').trim()
    sh "rm -f GIT_COMMIT"
    return gitCommit
}
	
node {
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/vish123al/vny-dcos.git'
   }
   stage('Build') {
      sh 'docker build -t vishaldenge/bg .'
   }
   stage('Push') 
   {
      sh "docker login -u vishaldenge -p 'v!sh@l123' " 
      sh 'docker push vishaldenge/bg'
   }
}
   
def getCurrentTarget() {
def currentTarget = readFile 'route-target'
sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/bmarathon.json'
           //return currentTarget 
  }
def getNewTarget() {
           def currentTarget = getCurrentTarget()
           def newTarget = ""
               if (currentTarget == 'tasks-blue') {
               newTarget = 'tasks-green'
	       sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/gmarathon.json'
	                                          } 
	       else if (currentTarget == 'tasks-green') {
               newTarget = 'tasks-blue'
	       sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/bmarathon.json'
		                                         } 
		   else { echo "OOPS, wrong target" }
  return newTarget
}
   
  

