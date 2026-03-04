pipeline {
    agent any

    stages {

        stage("SCM") {
            steps {
                git branch: 'main',
                    credentialsId: 'slave1',
                    url: 'https://github.com/Yahaya0338/maven-web.git'
            }
        }

        stage("Build") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Build Image") {
            steps {
                sh "docker build -t java-repo:${BUILD_NUMBER} ."
                sh "docker tag java-repo:${BUILD_NUMBER} yahayakhan/pipeline-java:${BUILD_NUMBER}"
            }
        }

        stage("Docker Login & Push") {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'DOCKER_PASS')]) {
                    sh """
                    echo \$DOCKER_PASS | docker login -u yahayakhan --password-stdin
                    docker push yahayakhan/pipeline-java:${BUILD_NUMBER}
                    """
                }
            }
        }

        stage("QAT Testing") {
            steps {
                sh "docker run -dit --name web100tom -p 8090:8080 yahayakhan/pipeline-java:${BUILD_NUMBER}"
            }
        }

        stage("Test Website") {
            steps {
                sh "sleep 20"
                sh "curl --ipv4 http://localhost:8090"
            }
        }
    }
}
