def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'npm', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
  node(label) {
    
      echo "Your Pipeline works!"
             sh('ls -la')

        stage('checkout'){
            def scmVars = checkout scm
            def commitHash = scmVars.GIT_COMMIT
        }

        stage('node') {
                    container('npm') {
                       sh('npm install')
                       sh('npm test')
                    }
        }

      stage('Create Docker images') {
      container('docker') {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
          credentialsId: 'dockerhub',
          usernameVariable: 'DOCKER_HUB_USER',
          passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
          sh """
            docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}
            docker build -t nasirumair/my-image .
            docker push nasirumair/my-image
            """
        }
      }
    }
    stage('Run kubectl') {
      container('kubectl') {
       // sh "kubectl run PLEASE_DEPLOY --image=nasirumair/my-image"
                sh "kubectl get pods"

      }
    }
  }
}
