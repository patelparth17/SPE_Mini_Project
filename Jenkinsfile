pipeline {
		agent any

	    environment {
            registry = "patelparth17/calcproj"
            registryCredential = 'dockerid'
            dockerImage = ''
        }

	stages {
        stage('Git Pull') {
            steps {
				git url: 'https://github.com/patelparth17/SPE_Mini_Project.git', branch: 'master',
                credentialsId: 'ansible'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Building the image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":latest"
                }
            }
        }
        stage('Deploying the image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps{
                sh "docker rmi $registry"
            }
        }

        stage('Ansible Deploy') {
            steps {
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'play.yml'

            }
        }
    }
}