node('java-slave-1') {
  stage('Prepare') {
   
      sh 'rm -rf NCE-WEB-TEST'
      sh "git clone ssh://git@g.hz.netease.com:22222/CloudQA/NCE-WEB-TEST.git"     
    
  }
  stage('check image’) {
      
  }
  stage('upgrade service') {

   }
  stage('ci test') {
  }

  def getOpenAPIToken(app_key, app_secret) {
    sh "curl  \
        -H "Content-Type: application/json" \
        -X POST -d '{"app_key":"${app_key}","app_secret":"${app_secret}"}'\
        http://10.180.156.17:10000/api/v1/token"
   }

 }
