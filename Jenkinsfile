pipeline {

    agent 'slave-jobs'
    stages {
        stage('SCM') {
             steps {
                git 'https://github.com/sr98877/pipeline-code.git'
            }

        }

        stage('Build by Maven Package') {
             steps {
		    sh 'sudo mvn dependency:purge-local-repository'	
                sh 'sudo mvn clean package'
            }

        }
        stage('Deploy on testing') {
             steps {
                // sh 'pwd'
                 sh "sudo docker build -t srronak/javatest-app:${BUILD_TAG} ."
                }
        }
        stage('Pushing image to docker hub') {
             steps {
                withCredentials([string(credentialsId: 'docker_hub_passwd', variable: 'Docker_hub_passwd_var')]) {

                sh "sudo docker login -u muditvyas26 -p ${docker_hub_passwd_var}"
                }
                sh "sudo docker push muditvyas26/javatest-app:${BUILD_TAG}"
        }

      }
      stage('Deploy on Docker Container') {
            steps {
                sh "sudo docker rm -f web1"
                sh "sudo docker rmi ${docker images -a q}"
                sh "sudo docker pull muditvyas26/javatest-app:${BUILD_TAG}"
                sh "sudo docker run -dit --name web1 -p 49153:8080 muditvyas26/javatest-app:${BUILD_TAG}"
                }
        }
     }
  }   
