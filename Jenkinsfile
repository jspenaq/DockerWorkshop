pipeline {
  agent any

  environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-jspenaq')
	}

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t jspenaq/minecraftserver:latest .'
      }
    }
    stage('Analyze') {
      steps {
        //  sh 'cd /home/ubuntu/docker-bench-security/'
        sh 'sudo sh docker-bench-security.sh'
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
    stage('Deploy') {
      steps {
        sh 'docker run -d -p 25565:25565 jspenaq/minecraftserver:latest'
      }
    }
  }

  post {
		always {
			sh 'docker logout'
		}
	}

}