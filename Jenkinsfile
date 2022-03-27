pipeline {
    agent any
options {
        timestamps()
        skipDefaultCheckout(true)
	}
    stages {
        /*
        stage('Configure docker') {
            steps {
                sh 'chmod 757 /var/run/docker.sock'
                sh 'docker run -v /var/run/docker.sock:/var/run/docker.sock -ti docker'
				}
			}
        */
        stage('Pull docker image') {
            steps {
                sh 'docker pull kiwenlau/hadoop:1.0'
				}
			}
        stage('Clone github repository') {
            steps {
                cleanWs()
                sh 'git clone https://github.com/kiwenlau/hadoop-cluster-docker'
				}
			}
        stage('Create hadoop network') {
            steps {
                script {
                    try {
                        sh 'docker network rm hadoop'
                        }
                    finally {
                    sh 'docker network create --driver=bridge hadoop'
                        }
				    }
                }
            }
        stage('Start container') {
            steps {
                sh 'ln -s /var/jenkins_home/workspace/hadoop-build_master/hadoop-cluster-docker ~/clusterPath'
                sh 'cd ~/clusterPath'
                sh 'ls -la'
                sh './start-container.sh'
				}
            }
        stage('Start hadoop') {
            steps {
                sh './start-hadoop.sh'
				}
            }
        stage('Run wordcount') {
            steps {
               sh './run-wordcount.sh'
				}
			}
		}
}
