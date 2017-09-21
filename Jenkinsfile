def gitCommit() {
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
        sh "docker login -u '${env.DOCKERHUB_USERNAME}' -p '${env.DOCKERHUB_PASSWORD}' -e vishaldenge1@gmail.com"
        sh "docker push vnyuser/vny:${gitCommit()}"
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
