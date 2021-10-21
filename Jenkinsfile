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
        sh 'git clone https://github.com/docker/docker-bench-security.git'
        sh 'cd docker-bench-security'
        sh 'docker-bench-security.sh'
        sh 'cd .. && rm -fr docker-bench-security'
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