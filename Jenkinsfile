pipeline {
    agent any

    stages {
        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('STUDENTAPI-REACT') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                TOMCAT_DIR="/Users/charanteja/Applications/apache-tomcat-10.1.44/webapps/reactstudentapi"

                if [ -d "$TOMCAT_DIR" ]; then
                    rm -rf "$TOMCAT_DIR"
                fi

                mkdir -p "$TOMCAT_DIR"
                cp -R STUDENTAPI-REACT/dist/* "$TOMCAT_DIR"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('STUDENTAPI-SPRINGBOOT') {
                    sh 'mvn clean package'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/charanteja/Applications/apache-tomcat-10.1.44/webapps"
                WAR_FILE="$TOMCAT_WEBAPPS/springbootstudentapi.war"
                WAR_DIR="$TOMCAT_WEBAPPS/springbootstudentapi"

                if [ -f "$WAR_FILE" ]; then
                    rm -f "$WAR_FILE"
                fi

                if [ -d "$WAR_DIR" ]; then
                    rm -rf "$WAR_DIR"
                fi

                cp STUDENTAPI-SPRINGBOOT/target/*.war "$TOMCAT_WEBAPPS/"
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}
