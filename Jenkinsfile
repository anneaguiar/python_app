pipeline {
  agent any
  environment {
    DB_HOST = 'localhost'  // Substitua pelo host do banco de dados real
    DB_USER = 'root'       // Substitua pelo usuário real
    DB_PASS = 'password'   // Substitua pela senha real
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: "${BRANCH_NAME}", url: 'https://github.com/<SEU_USUARIO>/sre-bootcamp-app.git'
      }
    }
    stage('Build') {
      steps {
        script {
          sh 'docker build -t myapp:${GIT_COMMIT} .'
        }
      }
    }
    stage('Test') {
      steps {
        script {
          sh 'docker run --rm myapp:${GIT_COMMIT} pytest'
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          if (env.BRANCH_NAME == 'prod') {
            // Deploy no ambiente de produção (exemplo: AWS EC2, ECS, etc.)
            sh 'docker run -d -p 80:80 myapp:${GIT_COMMIT}'
          }
        }
      }
    }
  }
  post {
    failure {
      mail to: 'youremail@example.com',
           subject: "Falha no deploy do commit ${GIT_COMMIT}",
           body: "O deploy da aplicação falhou."
    }
  }
}
