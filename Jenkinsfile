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
stage("Update the Deployment Tags") {
    steps {
        sh """
           echo "Before update:"
           cat deployment.yaml

          	 sed -i "s|\\(image: *mimaraslan/devops-application:\\).*|\\1${IMAGE_TAG}|" deployment.yaml

           echo "After update:"
           cat deployment.yaml
        """
    }
}



  // Yapılan güncellemeyi de  GitHub'a göndereceğiz.
stage("Push the changed deployment file to Git") {
  steps {
    withCredentials([usernamePassword(
      credentialsId: 'TOKEN_ID_GITHUB',             // Jenkins'teki ID (büyük/küçük harfe dikkat)
      usernameVariable: 'GIT_USER',
      passwordVariable: 'GIT_TOKEN'
    )]) {
      sh '''#!/usr/bin/env bash
                set -euo pipefail

                git config user.name  "mimaraslan"
                git config user.email "mimaraslan@gmail.com"

# (opsiyonel) detached HEAD ise branch'e geç
                git checkout -B master origin/master || true

                git add deployment.yaml || true

# değişiklik yoksa fail etme
                if git diff --cached --quiet; then
                    echo "No changes to commit."
                exit 0
                fi

                git commit -m "Updated deployment manifest"

# Token'ı URL'de kullan; Jenkins maskeler
                git push "https://${GIT_USER}:${GIT_TOKEN}@github.com/mimaraslan/devops_004_pipeline_gitops" HEAD:master
'''
    }
  }





    }
}