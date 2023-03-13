pipeline {
    agent any
	
	environment {
        AWS_ACCOUNT_ID="422097883691"
        AWS_DEFAULT_REGION="us-east-1"
    }

stages {    
 
    stage('Logging into AWS ECR') {
            steps {
                withAWS(credentials: 'sam-jenkins-demo-credentials', region: 'us-east-1'){
                // withCredentials([string(credentialsId: 'foo', variable: 'secret')]){
                    sh 'aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com'                  
                }
                 
            }
        }

    stage('Git clone') {
        steps{
            git branch: 'master', url: 'https://github.com/Alsafawagdy/MySQL-and-Python.git'
        }
    }

    stage('Build docker image Flask-app'){
        steps {
            sh 'docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/flask-app:v${BUILD_NUMBER} ./FlaskApp'
        }
    }

    stage('Build docker image MySQL-database'){
        steps {
            sh 'docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/mysql-db:v${BUILD_NUMBER} ./MySQL_Queries'
         }
    }

    stage('docker push Flask-app') {
        steps {
            withAWS(credentials: 'alsafa-final-iam', region: ${AWS_DEFAULT_REGION}){
            // withCredentials([string(credentialsId: 'foo', variable: 'secret')]){
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/flask-app:v${BUILD_NUMBER}'
            }
        }
    }

    stage('docker push MySQL-database') {
        steps {
            withAWS(credentials: 'alsafa-final-iam', region: 'us-east-1'){
            // withCredentials([string(credentialsId: 'foo', variable: 'secret')]){
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/mysql-db:v${BUILD_NUMBER}'
            }
        }
    }
    

    // stage('k8s-deploy-ingress-service') {
    //     steps {
    //         withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'k8s', contextName: '', 
    //            credentialsId: 'kubeconfig', namespace: '', serverUrl: 'https://172.31.21.73:6443']]) {
            
    //                     sh 'kubectl apply -f flaskapp-deployment.yml'
    //                     sh 'kubectl get pods '
    //         } 
    //     }
    // }

// stage('Deploying app to k8s') {
//   steps {

//     sshagent(['k8s']) {
//       sh "ssh -o StrictHostKeyChecking=no ansadmin@35.180.66.25 "
//       //sh "'scp /var/lib/jenkins/workspace/projet9/deployment-tomcat-to-k8s.yml ansadmin@35.180.66.25:./'"
//       sh "ssh -o StrictHostKeyChecking=no ansadmin@35.180.66.25  'ls'"

//       script {
//           sh "ssh ansadmin@35.180.66.25 kubectl create -f ."
//       }

//     }
//   }
// }  echo -n "password" |base64
// kubectl exec -n test-ebs -ti $MYSQLPOD -- mysql --user=root --password=password
// kubectl exec  -ti mysql-779df889dd-rkppb  -- mysql --user=dev --password=dev


}
}

