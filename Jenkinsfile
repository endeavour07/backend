pipeline {
    agent any

    tools {
        maven 'maven-3.9.9'
        jdk 'JAVA_HOME'
    }

    environment {
        BACKUP_DIR  = "D:\\OneDrive - WAISL LIMITED\\Desktop\\sac\\Be_Backend\\service-1"
        TOMCAT_HOME = "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1_Tomcat10.26"
        APP_NAME    = "service-1"
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
                    copy target\\*.war "%BACKUP_DIR%\\%APP_NAME%_%BUILD_NUMBER%.war"
                """
            }
        }

        stage('Deploy Latest WAR to Tomcat') {
            steps {
                echo "Deploying ONLY the latest WAR to Tomcat..."

                // Stop Tomcat
                bat "\"%TOMCAT_HOME%\\bin\\shutdown.bat\""

                // Remove old WAR (if any)
                bat """
                    if exist "%TOMCAT_HOME%\\webapps\\%APP_NAME%.war" (
                        del "%TOMCAT_HOME%\\webapps\\%APP_NAME%.war"
                    )
                    if exist "%TOMCAT_HOME%\\webapps\\%APP_NAME%" (
                        rmdir /S /Q "%TOMCAT_HOME%\\webapps\\%APP_NAME%"
                    )
                """

                // Copy ONLY latest WAR
                bat """
                    copy target\\*.war "%TOMCAT_HOME%\\webapps\\%APP_NAME%.war"
                """

                // Start Tomcat
                bat "\"%TOMCAT_HOME%\\bin\\startup.bat\""
            }
        }
    }

    post {
        success {
            echo "Latest WAR deployed successfully"
        }
        failure {
            echo "Deployment failed"
        }
    }
}