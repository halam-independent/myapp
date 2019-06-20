pipeline {
    agent any
    stages {
        stage ('Build') {
            steps{
                cd /home/tests/myapp
                npm install
                npm start
            }
        }
    }
}
