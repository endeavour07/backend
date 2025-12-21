pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java-11'
    }

    stages {

        stage('Pipeline Info') {
            steps {
                echo "Branch Name   : ${env.BRANCH_NAME}"
                echo "PR ID         : ${env.CHANGE_ID}"
                echo "Target Branch : ${env.CHANGE_TARGET}"
            }
        }

    }
}
