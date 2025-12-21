pipeline {
    agent any

    tools {
        maven 'maven-3.9.9'
        jdk 'JAVA_HOME'
    }

    environment {
        TOMCAT_HOME = "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1_Tomcat10.26"
        APP_NAME    = "service2"
        BACKUP_DIR  = "D:\\OneDrive - WAISL LIMITED\\Desktop\\sac\\Be_Backend\\service-2"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Backup WAR') {
            steps {
                bat """
                    if not exist "%BACKUP_DIR%" mkdir "%BACKUP_DIR%"
                    copy target\\${APP_NAME}.war "%BACKUP_DIR%\\${APP_NAME}_%BUILD_NUMBER%.war"
                """
            }
        }

        stage('Deploy to Tomcat (Hot Deploy)') {
            steps {
                echo "Hot deploying WAR without stopping Tomcat..."

                bat """
                    REM Remove old deployment
                    if exist "%TOMCAT_HOME%\\webapps\\${APP_NAME}" (
                        rmdir /S /Q "%TOMCAT_HOME%\\webapps\\${APP_NAME}"
                    )
                    if exist "%TOMCAT_HOME%\\webapps\\${APP_NAME}.war" (
                        del "%TOMCAT_HOME%\\webapps\\${APP_NAME}.war"
                    )

                    REM Deploy new WAR
                    copy target\\${APP_NAME}.war "%TOMCAT_HOME%\\webapps\\${APP_NAME}.war"
                """
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully (Hot Deploy)"
        }
        failure {
            echo "Deployment failed"
        }
    }
}