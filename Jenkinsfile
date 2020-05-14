pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World, my comment"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
    }
}