pipeline {
    agent { label 'demo'}
    
    stages {
        stage('Checkout git') {
            steps {
               git branch: 'main', url: 'https://github.com/praveensirvi1212/DevSecOps-project'
            }
        }
        
        stage ('Build & JUnit Test') {
            steps {
                sh 'mvn install' 
            }
            post {
               success {
                    junit 'target/surefire-reports/**/*.xml'
                }   
            }
        }
       // stage('SonarQube Analysis'){
         //   steps{
           //     withSonarQubeEnv('SonarQube-server') {
             //           sh 'mvn clean verify sonar:sonar \
              //          -Dsonar.projectKey=devsecops-project-key \
               //         -Dsonar.host.url=$sonarurl \
               //         -Dsonar.login=$sonarlogin'
              //  }
          //  }
       // }
       // stage("Quality Gate") {
         //   steps {
         //     timeout(time: 1, unit: 'HOURS') {
         //       waitForQualityGate abortPipeline: true
         //     }
         //   }
      //  }
        
        stage('Docker  Build') {
            steps {
      	        sh 'docker build -t praveensirvi/sprint-boot-app:v1.$BUILD_ID .'
                sh 'docker image tag praveensirvi/sprint-boot-app:v1.$BUILD_ID divyachintha/sprint-boot-app:v1.$BUILD_ID'
            }
        }
        //stage('Image Scan') {
         //   steps {
      	 //       sh ' trivy image --format template --template "@/usr/local/share/trivy/templates/html.tpl" -o report.html praveensirvi/sprint-boot-app:latest '
         //   }
       // }
        stage('Docker  Push'){
            environment {
                DOCKERHUB_CREDENTIALS = credentials('dockerhub')
                   }
            steps {
                // withVault(configuration: [skipSslVerification: true, timeout: 60, vaultCredentialId: 'vault-cred', vaultUrl: 'http://your-vault-server-ip:8200'], vaultSecrets: [[path: 'secrets/creds/docker', secretValues: [[vaultKey: 'username'], [vaultKey: 'password']]]]) {
                  //sh "docker login -u ${username} -p ${password} "
                 
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        
                    //sh 'docker push divyachintha/sprint-boot-app:v1.$BUILD_ID'
                    sh 'docker push divyachintha/sprint-boot-app:v1.$BUILD_ID'
                    sh 'docker rmi praveensirvi/sprint-boot-app:v1.$BUILD_ID divyachintha/sprint-boot-app:v1.$BUILD_ID'
              //  }
            }
        }
       stage('Deploy to k8s') {
         steps {
              script{
                kubernetesDeploy configs: 'spring-boot-deployment.yaml', kubeconfigId: 'kubernetes'
               }
           }
        }
    }  
 
   }
    //post{
      //  always{
        //    sendSlackNotifcation()
        //    }
       // }
//}

//def sendSlackNotifcation()
//{
  //  if ( currentBuild.currentResult == "SUCCESS" ) {
   //     buildSummary = "Job_name: ${env.JOB_NAME}\n Build_id: ${env.BUILD_ID} \n Status: *SUCCESS*\n Build_url: ${BUILD_URL}\n Job_url: ${JOB_URL} \n"
    //    slackSend( channel: "#devops", token: 'slack-token', color: 'good', message: "${buildSummary}")
    //}
   // else {
   //     buildSummary = "Job_name: ${env.JOB_NAME}\n Build_id: ${env.BUILD_ID} \n Status: *FAILURE*\n Build_url: ${BUILD_URL}\n Job_url: ${JOB_URL}\n  \n "
   //     slackSend( channel: "#devops", token: 'slack-token', color : "danger", message: "${buildSummary}")
   // }
//}

    
