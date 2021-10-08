pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: build
    image: 'maven:3.5.4-jdk-8-slim'
    command:
    - cat
    tty: true
    '''
            defaultContainer 'build'
  }
}
    tools{
        maven 'maven 3'
    }
    stages {
        stage ('build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
    }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage ('deploy') {
            steps {
                sh './scripts/deliver.sh'
                }
        }
        stage('Notify') {
            post {
                success {
                    mail to: bilal.hussain@concanon.com, subject: ‘The Pipeline was successful :)‘
            post {
                failure {
                    mail to: bilal.hussain@concanon.com, subject: ‘The Pipeline failed :(‘

                        }
                    }    
                }
            }
        }
    }
}