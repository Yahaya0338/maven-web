pipeline {
        agent any
        stages {
	        stage("SCM") {
                     steps {
                           git branch: 'main', credentialsId: 'slave1', url: 'https://github.com/Yahaya0338/maven-web.git'
                            }
                          }

                stage("build") {
                     steps {
                             sh 'sudo mvn clean package'
                             }
                           }
                stage("build-image") {
                     steps {
                             sh 'sudo docker build -t java-repo:$BUILD_TAG .'
                             sh 'sudo docker tag java-repo:$BUILD_TAG  yahayakhan/pipeline-java:BUILD_TAG'
                             }
                }
                stage("dockerlogin") {
                     steps { 
		              withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'dockerhub_pass')]) { 
			            sh 'sudo docker login -u yahayakhan -p $(dockerhub_pass)'
					        sh 'sudo docker push yahayakhan/pipeline-java:${BUILD _TAG}'
			             }
		            }
		      
		}
		stage("QAT TESTING") {
		      steps {  
		          sh 'sudo docker run -dit --name web100tom -p 8090:8080 yahayakhan/pipeline-java:$BUILD_TAG'
                    } 
	        }


	        stage("test-website") {
	             steps { 
		             sh 'sudo sleep 20'
		             sh 'sudo curl --ipv4 http://localhost:8090'

                }
	        }
}
}
