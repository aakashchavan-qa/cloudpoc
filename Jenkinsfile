pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhubstdin')
	}

	stages {

		stage('Build') {

			steps {
				sh 'mvn clean install'
				sh 'docker build -t akashnewscapepune/productimg:latest .'
			}
		}

		stage('Docker Hub Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push Image To Docker Hub') {

			steps {
				sh 'docker push akashnewscapepune/productimg:latest'
			}
		}
		stage('Deploy to K8s')
		{
			steps{
				sshagent(['k8s-jenkins'])
				{
					sh 'pwd'
					sh 'scp -r -o StrictHostKeyChecking=no /src/main/resources/deployment.yaml root@10.14.21.80:/root/Desktop/dockerImages/'
					
					script{
						try{
							//sh 'ssh root@10.14.21.80 kubectl apply -f /root/Desktop/dockerImages/deployment.yaml --kubeconfig=/root/.kube/kube.yaml'

							}catch(error)
							{

							}
					}
				}
			}
		}
		
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
