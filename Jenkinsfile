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

    
  }
}
