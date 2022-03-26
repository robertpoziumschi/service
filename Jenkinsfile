pipeline {
    agent any
    environment {
        DOCKER_PASSWORD = credentials("DOCKER_PASSWORD")
    }

    stages {
        stage('Build & Test') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Tag and build image') {
	      steps {
	        script {
	            GIT_TAG = sh([script: 'git fetch --tag && git tag', returnStdout: true]).trim()
	            MAJOR_VERSION = sh([script: 'git describe --tags --abbrev=0 | cut -d . -f 1', returnStdout: true]).trim()
	            MINOR_VERSION = sh([script: 'git describe --tags --abbrev=0 | cut -d . -f 2', returnStdout: true]).trim()
	            PATCH_VERSION = sh([script: 'git describe --tags --abbrev=0 | cut -d . -f 3', returnStdout: true]).trim()
	        }
        	sh "docker build -t robertpoziumschi/hello-img-pipeline:${MAJOR_VERSION}.\$((${MINOR_VERSION} + 1)).${PATCH_VERSION} ."
	      }
		}

		stage('Push image to dockerhub') {
	      steps {
	      	script {
	            GIT_TAG = sh([script: 'git fetch --tag && git tag', returnStdout: true]).trim()
	            MAJOR_VERSION = sh([script: 'git describe --tags --abbrev=0 | cut -d . -f 1', returnStdout: true]).trim()
	            MINOR_VERSION = sh([script: 'git describe --tags --abbrev=0 | cut -d . -f 2', returnStdout: true]).trim()
	            PATCH_VERSION = sh([script: 'git describe --tags --abbrev=0 | cut -d . -f 3', returnStdout: true]).trim()
	        }
	      	
        	sh "docker login docker.io -u robertpoziumschi -p ${DOCKER_PASSWORD}"
        	sh "docker push robertpoziumschi/hello-img-pipeline:${MAJOR_VERSION}.\$((${MINOR_VERSION} + 1)).${PATCH_VERSION}"
	      }
		}

		stage('Deploy to Kubernetes') {
	      steps {
        	sh "echo TODO"
	      }
		}
    }
}
