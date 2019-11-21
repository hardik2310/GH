pipeline {
    agent any
   //triggers {
    //   cron('*/2 * * * *')
    //}
    environment {
        USE_JDK = 'true'
        ABC = 'hello world'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                echo ABC 
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                echo 'environment path : '+env.PATH
                echo 'Build Id : '+env.BUILD_ID
                echo 'Build Number : '+env.BUILD_NUMBER
                echo 'Build Tag : '+env.BUILD_TAG
                echo 'Current Build Result : '+currentBuild.result
                echo 'Current Build Displayname : '+currentBuild.displayName
                //sh 'echo "Step 1"'
            }
        }
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                echo 'Deploying....'     
               // he()
            }
        }
    }
    post{
        always {
            echo 'always runs regardless of the completion status of the Pipeline run'
        }
        success {
            echo 'step will run only if the build is successful'
        }
        failure {
            echo 'only when the Pipeline is currently in a "failed" state run, usually expressed in the Web UI with the red indicator.'
            mail to: 'hardik.gosai@einfochips.com',
            subject: "Pipeline has failed: ${currentBuild.fullDisplayName}",
            body: "Error in ${env.BUILD_URL}"
        }
        unstable {
            echo 'current Pipeline has "unstable" state, usually by a failed test, code violations and other causes, in order to run. Usually represented in a web UI with a yellow indication.'
        }
        changed {
            echo 'can only be run if the current Pipeline is running at a different state than the previously completed Pipeline'
        }
    }
}
node {
    stage('SampleTryCatch') {
        try {
            sh 'exit 1'
        }
        catch (exc) {
             echo "Something didn't work and got some exceptions"
         }
     }
 } 
def he(){
    echo 'he function'
    def auto_job = build job: JOB_NAME , parameters: [string(name: 'PullReqId', value: "${env.CHANGE_ID}"), 
                                                         string(name: 'PR_NAME', value: "${env.BRANCH_NAME}")], propagate: false
    echo 'build over'
    result = auto_job.result   
    
    echo 'abc'
}
