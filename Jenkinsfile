pipeline {
  agent any

  environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-jspenaq')
	}

  stages {
    // stage('Prepare') {
    //   steps {
    //     sh 'npm --version'
    //     sh 'npm ci'
    //   }
    // }
    stage('Build') {
      steps {
        sh 'docker build -t jspenaq/minecraftserver:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push jspenaq/minecraftserver:latest'
      }
    }
    // stage('Deploy') {
    //   steps {
        // sh 'docker run'
    //   }
    // }
  }

  post {
		always {
			sh 'docker logout'
		}
	}

}