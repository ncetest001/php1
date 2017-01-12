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
        sh 'echo hello2 >> yuz2'
   }
  stage('ci test') {
       def token = getCombToken("3e4321b66be945a48599eeaa53099057","4c6f9a7a37a942529adb526a4a0114b0")
       def getCombImage(token,ci)
  }
}
    def getCombToken(app_key, app_secret) {
      def combURL = 'http://115.238.123.127:10000/api/v1/token'
      def header  = 'Content-Type:application/json'
      def payload = JsonOutput.toJson([app_key      : app_key,
                                       app_secret   : app_secret])
      sh "curl -X POST -H \'${header}\' -d \'${payload}\' ${combURL} > json"
      sh 'cat json |grep -Po \'(?<="token":")[^"]*\' > token'
      def token=readFile('token').trim()
      echo "$token"
     }
    def getCombImage(token, repoName) {
      def combURL = 'http://115.238.123.127:10000/api/v1/microservices/images'
      def header  = 'Authorization:Token ${token}'
      sh "curl -X POST -H \'${header}\' -d \'${payload}\' ${combURL}"
     }
