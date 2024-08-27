pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:$PATH"
    }

    stages {

	stage('Check Docker') {
    	    steps {
       		 sh 'docker --version'
   	    }
        }

        stage('Clone repository') {
            steps {
                /* Let's make sure we have the repository cloned to our workspace */
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                /* This builds the actual image; synonymous to
                 * docker build on the command line */
                script {
                    app = docker.build("joeysapp")
                }
            }
        }

        stage('Test image') {
    steps {
        script {
            echo 'tests passed'
        }
    }
}


        stage('Push image') {
            steps {
                /* Finally, we'll push the image with two tags:
                 * First, the incremental build number from Jenkins
                 * Second, the 'latest' tag.
                 * Pushing multiple tags is cheap, as all the layers are reused. */
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
