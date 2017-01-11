node('java-slave-1') {
  stage('Prepare') {
      sh 'rm -rf NCE-WEB-TEST'
      sh "git clone https://github.com/sanqianaa/apitest1"     
  }
  stage('check image') {
        sh 'echo hello1 >> yuz1'
  }
  stage('upgrade service') {
        sh 'echo hello2 >> yuz2'
   }
  stage('ci test') {
        getOpenAPIToken("3e4321b66be945a48599eeaa53099057","4c6f9a7a37a942529adb526a4a0114b0")
  }
}
  def getOpenAPIToken(app_key, app_secret) {
    sh "curl -H 'Content-Type:application/json' -X POST -d '{"app_key":"${app_key}","app_secret":"${app_secret}"}' http://10.180.156.17:10000/api/v1/token"
   }
