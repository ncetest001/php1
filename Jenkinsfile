import groovy.json.JsonOutput
node('java-slave-4') {
      def token = getCombToken("3e4321b66be945a48599eeaa53099057","4c6f9a7a37a942529adb526a4a0114b0")
      originTag = getCombImageLatestTag(token,"ci")
      println("The original image tag is : "+originTag)
  stage('Prepare') {
      sh 'pwd'
      sh 'apt-get update -y ; apt-get install jq'
      sh 'rm -rf hooktest'
      sh "git clone ssh://yu.zhang@git.hz.netease.com:22222/yu.zhang/hooktest.git"  
  }
  stage('check image') {
    timeout(600){
       tag = getCombImageLatestTag(token,"ci") 
             println(Tag)
    }
  }
  stage('upgrade service') {
        sh 'echo hello2 >> yuz2'
   }
  stage('ci test') {
        sh 'cd /home/jenkins/workspace/apitest1/hooktest/NCE-WEB-TEST/'
        sh 'mvn clean -f hooktest/NCE-WEB-TEST/pom.xml test -Dmaven.test.failure.ignore=true -DsuitXmlFile=./hooktest/NCE-WEB-TEST/src/test/resources/xml/microserviceopenapi.xml'
  }
}
    def String getCombToken(app_key, app_secret) {
      def combTokenURL = 'http://115.238.123.127:10000/api/v1/token'
      def header = 'Content-Type:application/json'
      def payload = JsonOutput.toJson([app_key      : app_key,
                                       app_secret   : app_secret])
      sh "curl -X POST -H \'${header}\' -d \'${payload}\' ${combTokenURL} > json"
      sh 'cat json |grep -Po \'(?<="token":")[^"]*\' > token'
      def token = readFile('token').trim()
      return token
     }
    def String getCombImageLatestTag(token, repoName) {
      def combGetImageURL = 'http://115.238.123.127:10000/api/v1/microservices/images'
      def header  = "Authorization:Token ${token}"
      sh "curl -H \'${header}\'  ${combGetImageURL} > json"
      return sh 'jq \'.[][] | select(.repo_name=="ci") | .tag\' json |  sed -n \'1p\' '
     }
    def createCombService(imagePath) {
      def combCreateServiceURL= 'http://115.238.123.127:10000/api/v1/microservices'
      def header = 'Authorization:Token ${token}'
      sh "curl -X POST -H \'${header}\' -d  \'${payload}\' ${combTokenURL} > json"
    }
    def getCombService(id) {
      def combCreateServiceURL= 'http://115.238.123.127:10000/api/v1/microservices'
      def header = 'Authorization:Token ${token}'
      sh "curl -X POST -H \'${header}\' -d  \'${payload}\' ${combTokenURL} > json"
    }
    def deleteCombService(id) {
      def combDeleteServiceURL= 'http://115.238.123.127:10000/api/v1/microservices/${id}'
      def header = 'Authorization:Token ${token}'
      sh "curl -X DELETE -H \'${header}\' ${combTokenURL} > json"
    }
