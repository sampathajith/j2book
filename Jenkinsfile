pipeline {
    agent any // Uses any available node instead of specific worker_nodes

    stages {
        stage('Source') {
            steps {
                deleteDir() // Standard version of cleanupWs
                checkout scm
                stash name: 'test-sources', includes: 'build.gradle,src/test/'
            }
        }
        stage('Build') {
            steps {
                // Using standard shell instead of custom gbuild2
                sh './gradlew clean build -x test' 
            }
        }
        stage('Test') {
            parallel {
                stage('Test Set 1') {
                    steps {
                        unstash 'test-sources'
                        sh './gradlew -Dtest.single=TestExample1 test'
                    }
                }
                stage('Test Set 2') {
                    steps {
                        unstash 'test-sources'
                        sh './gradlew -Dtest.single=TestExample2 test'
                    }
                }
            }
        }
    }
    post {
        always {
            echo "Finished build for sampath.a007@gmail.com"
        }
    }
}
