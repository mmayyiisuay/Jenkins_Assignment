pipeline {
  agent {
    label 'vm2'
  }

  stages {
    stage('Clone') {
      steps {
        git branch: 'main', url: 'https://github.com/mmayyiisuay/Jenkins_Assignment.git'
        sh 'whoami'
      }
    }
    stage('Install Packet') {
      steps {
        sh 'npm install'
      }
    }
    stage('Run Unittest') {
      steps {
        sh 'whoami'
        sh 'npm test'
      }
    }
    stage('Run Robot') {
      steps {
        echo 'Create Container'
        sh 'docker compose -f ./docker-compose.dev.yaml up -d --build'
        echo 'Cloning Robots'
        dir('./robot/') {
          git branch: 'main', url: 'https://github.com/softdev-practice-kmitl/Jenkins_Robot.git'
        }
        echo 'Runing Robot'
        sh 'cd ./robot && python3 -m robot ./test-api.robot'
      }
    }
    stage('Building Image ️') {
      steps {
        sh 'docker build -t mmayyiisuay/jenkins-assingment:lastest .'
      }
    }
    stage('Push ⬆️') {
      steps {
        sh 'docker push mmayyiisuay/jenkins-assingment:lastest'
      }
    }
    stage('Clean Workspace') {
      steps {
        echo 'DownTime'
        sh 'docker compose -f ./docker-compose.dev.yaml down'
        sh 'docker system prune -a -f'
      }
    }
    stage('Running Preprod') {
      agent {
        label 'vm3'
      }
      steps {
        sh 'docker compose down && docker system prune -a -f && docker compose up -d --build'
      }
    }
  }
}