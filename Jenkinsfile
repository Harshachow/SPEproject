pipeline {
  agent any
  stages {
    stage('Build'){
    steps{
      script{
      sh 'npm install'
      }
    }
    }
    stage('Docker Image Build') {
      steps {
        script {
          sh 'docker-compose build'
        }

      }
    }
    stage('Docker push') {
      steps {
        script {
          withDockerRegistry([ credentialsId: "DockerHub", url: "" ])
          {
            sh 'docker push harsha9199/db:latest'
            sh 'docker push harsha9199/redis:latest'
            sh 'docker push harsha9199/ipl:latest'
            
          }
        }

      }
    }
    stage('Deploy with Rundeck') {
      agent any
      steps {
        script {
          step([$class: "RundeckNotifier",
          rundeckInstance: "Rundeck",
          shouldFailTheBuild: true,
          shouldWaitForRundeckJob: true,
          options: """
            BUILD_VERSION=$BUILD_NUMBER
          """,
          jobId: "b0a4af41-b909-43be-b39f-1db2b3c82b04"])
        }
      }
    }
    // stage('Test') {
    //   steps {
    //       sh 'selenium-side-runner --output-directory=./testing/results -c "browserName=chrome goog:chromeOptions.args=[headless]" --output-format=junit ./testing/IIITB-Event-Calendar.side'
    //   }
    // }
  }
}
