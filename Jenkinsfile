node('java-slave-1') {
  stage('Prepare') {
      sh 'rm -rf hooktest'
      sh "git clone ssh://yu.zhang@git.hz.netease.com:22222/yu.zhang/hooktest.git"     
  }
  stage('check image') {
        sh 'echo hello1 >> yuz1'
        sh 'pwd'
  }
  stage('upgrade service') {
        sh 'echo hello2 >> yuz2'
   }
  stage('ci test') {
        sh "curl -H 'Content-Type:application/json' -X POST -d '{"app_key":"${app_key}","app_secret":"${app_secret}"}' http://115.238.123.127:10000/api/v1/token"
  }
}
  def getOpenAPIToken(app_key, app_secret) {
    sh "curl -H 'Content-Type:application/json' -X POST -d '{"app_key":"${app_key}","app_secret":"${app_secret}"}' http://115.238.123.127:10000/api/v1/token"
   }
