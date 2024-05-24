node {
     stage('Clone repository') {
         checkout scm
     }
     stage('Build image') {
         app = docker.build("admin/flask-example")
         
     }
     stage('Push image') {
         docker.withRegistry('https://127.0.0.1/', 'harbor_cred') {
             app.push("${env.BUILD_NUMBER}")
             app.push("latest")
         }
     }
     stage('SonarQubeScanner') {
         def scannerHome = tool 'SonarQubeScanner';
         withSonarQubeEnv('SonarQubeScanner') {
             sh """${scannerHome}/bin/sonar-scanner \
                             -Dsonar.projectKey=demo-project \
                             -Dsonar.sources=. \
                             -Dsonar.host.url=http://10.0.2.15:9000 \
                             -Dsonar.login=squ_d1b2b47f523810c04e8b4456fb1634ab4c7b791c
             """
        }
     }
}
