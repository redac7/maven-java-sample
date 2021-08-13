pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Test'
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                echo 'Package'
                sh 'mvn package'
            }
            post {
                always {
                    echo 'Post Test results'
                    junit 'target/surefire-reports/*.xml'
                }
                success {
                    echo 'Archive Artifacts'
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy'
                withEnv(['JENKINS_NODE_COOKIE=dontkillMe']){
                    sh 'nohup java -jar -DServer.port=8001 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }
        }
    }
}
