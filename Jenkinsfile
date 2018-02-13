node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "192.168.80.180:5000/demo/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    stage "Push"
        sh "docker login 192.168.80.180:5000 -u admin -p Harbor12345"
        sh "docker push ${imageName}"

    stage "Deploy"

        sh "sed 's#192.168.80.180:5000/demo/hello-kenzan:latest#'$BUILDIMG'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply -f -"
        //sh "kubectl apply -f applications/hello-kenzan/k8s/deployment.yaml"

        sh "kubectl rollout status deployment/hello-kenzan"
}