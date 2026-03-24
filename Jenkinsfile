pipeline {
    agent { label 'ubuntu-agent' }

    environment {
        APP_NAME = "multi-branch-project"
        REPO_URL = "https://github.com/hridyen/differentbranch-singlepipeline.git"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: "${env.BRANCH_NAME}",
                    url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${APP_NAME}:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                script {

                    if (env.BRANCH_NAME == "dev") {
                        sh """
                        docker rm -f ${APP_NAME}-dev || true
                        docker run -d -p 3004:3000 \
                        -e BRANCH=dev \
                        --name ${APP_NAME}-dev \
                        ${APP_NAME}:dev
                        """
                    }

                    if (env.BRANCH_NAME == "prod") {
                        sh """
                        docker rm -f ${APP_NAME}-prod || true
                        docker run -d -p 3003:3000 \
                        -e BRANCH=prod \
                        --name ${APP_NAME}-prod \
                        ${APP_NAME}:prod
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "SUCCESS for ${env.BRANCH_NAME}"
        }
        failure {
            echo "FAILED for ${env.BRANCH_NAME}"
        }
    }
}