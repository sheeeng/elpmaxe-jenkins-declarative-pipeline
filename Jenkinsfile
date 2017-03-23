pipeline {
    agent {
        // `agent { node { label 'labelName' } }` behaves the same as
        // `agent { label 'labelName' }, but `node` allows for
        // additional options (such as customWorkspace).
        node {
            label 'linux'
        }
    }
    environment {
        // https://github.com/jenkinsci/pipeline-model-definition-plugin/wiki/Environment-variables
        LOREM = "lorem"
        IPSUM = "ipsum"
    }
    options {
        // http://javadoc.jenkins.io/hudson/tasks/LogRotator.html
        buildDiscarder(logRotator(daysToKeepStr: '3', numToKeepStr: '5'))
        disableConcurrentBuilds()
        // skipDefaultCheckout()
        retry(3)
        timeout(time: 1, unit: 'HOURS')
    }
    tools {
        // The tool name must be pre-configured in Jenkins.
        // Under Manage Jenkins → Global Tool Configuration menu.
        maven 'M3'
        jdk 'JDK8'
    }
    triggers {
        // “At 22:00 on every day-of-week from Monday through Friday.”
        cron('0 22 * * 1-5')

        // “At every 5th minute past every hour from 7 through 23
        // on every day-of-week from Monday through Saturday.
        pollSCM('*/5 7-23 * * 1-6')
    }
    stages {
        stage('Preparation') {
            steps {
                echo 'Preparation stage called.'

                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''

                script {
                    git url: 'https://github.com/jglick/simple-maven-project-with-tests.git'

                    // https://jenkins.io/doc/book/pipeline/syntax/#script
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Mock call the ${browsers[i]} browser."
                    }

                    // List all `env` properties within Jenkins Pipeline.
                    // http://stackoverflow.com/a/42138466
                    echo sh(
                        returnStdout: true,
                        script: 'env'
                    )
                    echo sh(
                        returnStdout: true,
                        script: 'echo \u2600 \u2620 \
                            \u2776 \u2777 \u2778 \u2779'
                    )
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Build stage called.'
                script {
                    // https://jenkins.io/doc/book/pipeline/jenkinsfile/#advanced-scripted-pipeline
            parallel linux: {
                    node('linux') {
                            try {
                                // sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
                                sh "mvn -Dmaven.test.failure.ignore clean package"
                            }
                            finally {

                            }
                        }
                    },
                    windows: {
                        node('windows') {
                            // bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
                            bat(/mvn -Dmaven.test.failure.ignore clean package/)
                        }
                    }
                }
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
                script {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archive 'target/*.jar'
                }
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
