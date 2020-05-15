pipeline {
    agent any
     environment{
        CHECK_URL = "https://static-udacity-jenkins-pipeline.s3.us-east-2.amazonaws.com/index.html"
        CMD = "curl --write-out %{http_code} --silent --output /dev/null ${CHECK_URL}"

    }
    stages {
        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Upload to AWS') {
            steps {
                withAWS(region:'us-east-2',credentials:'282457606471') {
                    sh 'echo "Uploading content with AWS creds"'
                    s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-udacity-jenkins-pipeline')
                }
            }
        }
        stage('Stage-One') {
            steps {
                script{
                    sh "${CMD} > commandResult"
                    env.status = readFile('commandResult').trim()
                }
            }
        }
        stage('Stage-Two') {
            steps {
                script {
                    sh "echo ${env.status}"
                    if (env.status == '200') {
                        currentBuild.result = "SUCCESS"
                    }
                    else {
                        currentBuild.result = "FAILURE"
                    }
                }
            }
        }
    }
}