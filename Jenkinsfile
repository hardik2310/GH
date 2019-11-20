pipeline {
    agent any
    triggers {
        cron('H */2 * * 1-3')
    }
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
                echo env.PATH
                echo env.BUILD_ID
                echo env.BUILD_NUMBER
                echo env.BUILD_TAG
                echo currentBuild.result
                echo currentBuild.displayName
                sh 'echo "Step 1"'
                @def auto_job = build job: jobName , parameters: [string(name: 'PullReqId', value: "${env.CHANGE_ID}"), 
                                                         string(name: 'PR_NAME', value: "${env.BRANCH_NAME}"),
                                                         string(name: 'CSU_branch', value: "master"), //Comment to not use common CSU_branch
                                                         string(name: 'CTU_branch', value: "master"), // Comment to not use common CSU_branch
                                                         string(name: 'Router_SSID', value: "wseco1_netgear-5G"),
                                                         string(name: 'Router_password', value: "04042018")], propagate: false
                @result = auto_job.result
                echo result
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
                he()
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
    echo 'hello'
}
