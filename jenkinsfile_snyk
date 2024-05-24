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
    stage('Test') {
        echo 'Testing...'
        snykSecurity(
          snykInstallation: 'snyk@latest', //<Your Snyk Installation Name>
          snykTokenId: 'snyk-api-token', //<Your Snyk API Token ID>
          monitorProjectOnBuild: false, // 모니터링 여부
          failOnIssues: false, // 취약성 발견시 빌드 실패로 할지여부(기본 값: true)  실패시에도 보고서는 생성됨
          // place other parameters here
          additionalArguments: 'https://github.com/gasbugs/flask-example'
        )
    }
}
