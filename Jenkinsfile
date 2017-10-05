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
  // stage('Prepare Scripts') 
  // {
     // sh 'sed -i \'s/blue/\'${BUILD_ID}\'/g\' json/bmarathon.json'
     // sh 'sed -i \'s/green/\'$((${BUILD_ID}-1))\'/g\' json/gmarathon.json'
    //  sh 'sed -i \'s/IDTAG/\'${BUILD_ID}\'/g\' deploy/updategw50.yaml'
     // sh 'sed -i \'s/IDPRETAG/\'$((${BUILD_ID}-1))\'/g\' deploy/updategw50.yaml'
     // sh 'sed -i \'s/IDTAG/\'${BUILD_ID}\'/g\' deploy/updategw100.yaml'
     // sh 'sed -i \'s/IDPRETAG/\'$((${BUILD_ID}-1))\'/g\' deploy/updategw100.yaml'
  // } 
  // def x = 0;
   //def y = 0;
   stage('Deploy in Cluster') 
   {
	   input 'Do you approve deployment for blue?'
       sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/bmarathon.json'
	   input 'Do you approve deployment for green?'
	   sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/gmarathon.json'
   }
   stage('Move GW 50/50') 
   {
       input 'Do you approve deployment for blue?'
       sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/bmarathon.json'
	   input 'Do you approve deployment for green?'
	   sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/gmarathon.json'
   }
   stage('Move Full') 
   {
       input 'Do you approve deployment for blue?'
       sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/bmarathon.json'
	   input 'Do you approve deployment for green?'
	   sh 'curl -X POST -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@json/gmarathon.json'
   }
   stage('Undeploy Previous App') 
   {
       input 'Do you want to delete green old deplyment?'
       sh 'curl -X DELETE -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@gmarathon.json'
	   input 'Do you want to delete old blue deployment?'
	   sh 'curl -X DELETE -H "Content-Type: application/json" http://10.0.1.85:8080/v2/apps -d@bmarathon'

	   }
}
