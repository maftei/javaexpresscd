pipeline {

  environment {
    app = "webapp"
    environment = "dev"
    namespace = "default"
  }
  agent any

    stages {

      stage ('Checkout SCM'){
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/dev']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/maftei/javaexpresscd.git']]])
        }
      }

	  stage ('Deploy Helm Charts')  {
	      steps {

           dir("charts"){
             withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                    sh 'sudo /usr/local/bin/helm repo add rmaftei-helm-local  https://jfrogest.jfrog.io/artifactory/rmaftei-helm-local/ --username $username --password $password'
                    sh "sudo /usr/local/bin/helm repo update"
                    sh "sudo /usr/local/bin/helm upgrade ${app}-${environment} --install --namespace ${namespace} --force -f values.yaml ."
                    sh "sudo /usr/local/bin/helm list -a --namespace ${namespace}"
                }
           }
        }

      }

   }
}