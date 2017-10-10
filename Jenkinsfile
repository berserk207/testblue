pipeline {
    agent any 

    stages {
        stage('Build') { 
            steps { 
                echo 'build'                
                checkout scm
            }
        }
        stage('Test'){
            steps {
                echo 'test'
                sh 'php -l tutorial/protected'
            }
        }

        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploy'                
                sshagent (credentials: ['d292a665-8f05-4e56-96e1-f2460330d2be']) {
                    sh 'ssh -o StrictHostKeyChecking=no -l ph_deployer 133.242.62.103 uname -a'
                }
                
            }
        }
    }

    post {
        success {
            echo 'build successful...'
            sh "python ${env.WORKSPACE}/chatwork_msg.py --message 'Build successful' --groupid 64322215"
        }

    }
}
