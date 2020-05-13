#!/usr/bin/env groovy
pipeline {
    agent any
   //triggers {
    //   cron('*/2 * * * *')
    //}
    environment {
        USE_JDK = 'true'
        ABC = 'environment variable ABC'
    }
   
    stages {
        stage('Run Tests') {
            parallel {
                stage('Test On Windows') {
                    agent none
                    steps {
                        echo "steps"
                        println "for run test"
                    }
                
                    post {
                        always {
                            echo "always run for windows"
                        }
                    }
                }
                stage('Test On Linux') {
                    agent none
                    steps {
                        echo "steps"
                    }
                    post {
                        always {
                            echo "always run for linux"
                        }
                    }
                }
            }
        }   
        stage('Build') {
            steps {
                echo 'Building..'
                echo ABC 
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        echo "hello"
                    }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                echo 'environment path : '+env.PATH
                echo 'Build Id : '+env.BUILD_ID
                echo 'Build Number : '+env.BUILD_NUMBER
                echo 'Build Tag : '+env.BUILD_TAG
                input('Do you want to proceed?')
                //sh 'echo "Step 1"'
            }
        }
        stage('Deploy') {
            when {
              expression {
                 currentBuild.result!='SUCCEESS'
              }
            }
            steps {
                echo 'Deploying....'   
                he()
            }
        }
    }
    post{
        always {
            echo 'always runs'
            echo 'Current Build Result : '+currentBuild.result
            echo 'Current Build Displayname : '+currentBuild.displayName
            echo env.CHANGE_ID
            echo env.BRANCH_NAME
        }
        success {
            echo 'step will run only if the build is successful'
        }
        failure {
            echo 'only when the Pipeline is currently in a "failed" state run.'
            //mail to: 'hardik.gosai@einfochips.com',
            //subject: "Pipeline has failed: ${currentBuild.fullDisplayName}",
            //body: "Error in ${env.BUILD_URL}"
        }
        unstable {
            echo 'current Pipeline has "unstable" state.'
        }
        changed{
            echo "output is changed.."
        }
    }
}
${BUILD_URL}/consoleText
node {
    try {
        lock(resource: null, variable: 'LOCKED_RESOURCE') {
            echo env.LOCKED_RESOURCE
            stage('SampleTryCatch') {
                step{
                    sh 'exit 1'
                }
            }
        }
    }
    catch (exc) {
        echo "Something didn't work and got some exceptions"
    }
}
def he(){
    echo 'he function'
    
    echo 'abc'
}
