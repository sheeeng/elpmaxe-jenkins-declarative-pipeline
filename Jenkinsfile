pipeline {
    agent { 
        // `agent { node { label 'labelName' } }` behaves the same as
        // `agent { label 'labelName' }, but `node` allows for
        // additional options (such as customWorkspace).
        node { 
            label 'linux'
        }
    }
    tools {
        // The tool name must be pre-configured in Jenkins.
        // Under Manage Jenkins → Global Tool Configuration menu.
        maven 'M3'
    }
    stages {
        stage('Preparation') {
            steps {
                echo 'Preparation stage called.'
            }
        }
        stage('Build') {
            steps {
                echo 'Build stage called.'
            }
        }
        stage('Analysis') {
            steps {
                echo 'Analysis stage called.'
            }
        }
        stage('Results') {
            steps {
                echo 'Results stage called.'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy stage called.'
            }
        }
    }
    post {
        always {
            echo 'This will always run.'
        }
        success {
            echo 'This will run only if successful.'
        }
        failure {
            echo 'This will run only if failed.'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable.'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed.'
            echo 'For example, if the Pipeline was previously failing but is now successful.'
        }
    }
}
