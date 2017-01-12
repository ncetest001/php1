import groovy.json.JsonOutput
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
        notifySlack("a","a")
        sh 'echo hello2 >> yuz2'
   }
  stage('ci test') {
        getOpenAPIToken2("3e4321b66be945a48599eeaa53099057","4c6f9a7a37a942529adb526a4a0114b0")
  }
}
  def getOpenAPIToken(app_key, app_secret) {
    sh "curl -H \'Content-Type:application/json\' -X POST -d \'{"app_key":"${app_key}","app_secret":"${app_secret}"}\' http://115.238.123.127:10000/api/v1/token"
   }

    def getOpenAPIToken2(app_key, app_secret) {
    def combURL = 'http://115.238.123.127:10000/api/v1/token'
    def payload = JsonOutput.toJson([app_key      : app_key,
                                     app_secret   : app_secret])
    sh "curl -X POST --data-urlencode \'payload=${payload}\' ${combURL}"
     }
