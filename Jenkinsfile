node() {
    echo "Your Pipeline works!"
         sh('ls -la')

    stage('checkout'){
        def scmVars = checkout scm
        def commitHash = scmVars.GIT_COMMIT
    }
    
    stage('node') {
             agent {
                docker { image 'node:carbon-jessie' }
            }
            steps {
                npm install
                npm test
            }
    }

}
