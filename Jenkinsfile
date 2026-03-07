pipeline {
    agent {
        node {
            label 'My-Jenkins-Agent'
        }
    }

    environment {
          APP_NAME = "devops-application-gitops"
    }



    stages {

        stage('Clean Workspace') {
                steps {
                cleanWs()
            }
        }

        stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mimaraslan/devops_004_pipeline_gitops']])
            }
        }

      


  // Deployment kısmında güncelleme


  // Yapılan güncellemeyi de  GitHub'a göndereceğiz.



    }
}