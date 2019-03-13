node() {
    echo "Your Pipeline works!"
         sh('ls -la')

    stage('checkout'){
        def scmVars = checkout scm
        def commitHash = scmVars.GIT_COMMIT
    }
    
    stage('test-npm') {
        npm install
        npm test
    }
}
