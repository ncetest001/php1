import groovy.json.JsonOutput
node('java-slave-4') {
      def repoName = "ci"
      def microserviceId = 
      def container_id = 
      def token = getCombToken("3e4321b66be945a48599eeaa53099057","4c6f9a7a37a942529adb526a4a0114b0")
      def originPath = getCombImageLatestPath(token,"ci")
      def newPath 
      println("##The original image path is : "+originPath)
//准备测试代码与工具      
  stage('prepare') {
      sh 'pwd'
//      sh 'apt-get update -y ; apt-get install jq'
      sh 'rm -rf hooktest'
      sh "git clone ssh://yu.zhang@git.hz.netease.com:22222/yu.zhang/hooktest.git"  
  }
//等待由gitpush触发的镜像构建完成
  stage('check image') {
       def boolean isready = waitImageReady(token,repoName,originPath)  	
    if(isready){  
	  return newPath = getCombImageLatestPath(token,"ci")
    }else{
	  println("##Failed to get the image")
    }
  }
//将最新的镜像部署成服务
  stage('upgrade service') {
	createCombService(microserviceId)
	waitCombServiceReady(token,microserviceId)
  }
//等待服务创建成功
//执行测试      
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
          
    def String getCombImageLatestPath(token, repoName) {
      def combGetImageURL = 'http://115.238.123.127:10000/api/v1/microservices/images'
      def header  = "Authorization:Token ${token}"
      sh "curl -H \'${header}\'  ${combGetImageURL} > json"
      sh 'jq -r \'.[][] | select(.repo_name=="ci") | .image_path\' json |  sed -n \'1p\' > image_path'
      def image_path = readFile('image_path').trim()
      return image_path
     }
          
    def createCombService(imagePath) {
      def combCreateServiceURL= 'http://115.238.123.127:10000/api/v1/microservices'
      def header = "Authorization:Token ${token}"
      sh "curl -X POST -H \'${header}\' -d  \'${payload}\' ${combTokenURL} > json"
    }

    def updateCombServiceImage(microserviceId,container_id,image_path)
      def combCreateServiceURL= "http://115.238.123.127:10000 /api/v1/microservices/${microserviceId}/actions/update-image"
      def header = "Authorization:Token ${token}"
      def payload = JsonOutput.toJson([min_ready_seconds      : "20",
                                       container_images   : [{
					      "container_id":${container_id},
					      "image_path": ${image_path}] 
				      ])
      sh "curl -X PUT -H \'${header}\' -d \'${payload}\' ${combTokenURL} > json"
    }
          
    def String getCombServiceStatus(microserviceId) {
      def combCreateServiceURL= "http://115.238.123.127:10000/api/v1/microservices/${microserviceId}"
      def header = "Authorization:Token ${token}"
      sh "curl  -H \'${header}\'  ${combTokenURL} > json"
      sh 'cat json | jq -r .service_info.status'
      def status = readFile('json').trim()
      return status
    }
    def String deleteCombService(microserviceId) {
      def combDeleteServiceURL= "http://115.238.123.127:10000api/v1/microservices/${microserviceId}?free_ip=false"
      def header = "Authorization:Token ${token}"
      sh "curl -X DELETE -H \'${header}\' ${combTokenURL} > json"  
      sh 'cat json | jq .code >　code'
      def code = readfILE.('code').trim()
      return code
    }
    def boolean waitImageReady(token,repoName,originPath){
            long startTime = System.currentTimeMillis()
		boolean flag = false
		try {
		      while (System.currentTimeMillis() - startTime < 60*10*1000) {
		      theLatestPath = getCombImageLatestPath(token,repoName)
		          if (theLatestPath != originPath){
				      println("##The new image is ready !!")
                              	      flag = true
				      break
		          } else {
				      println("##The new image is not ready")
                              	      sleep 10				
				 }
		       }
	    	  if (false == flag)
   		     println("##Image is timeout")
		} catch (Exception e) {
			println(e)
			flag = false
		} 
	    return flag
    }
    def boolean waitCombServiceReady(token,microserviceId){
            long startTime = System.currentTimeMillis()
		boolean flag = false
		try {
		      while (System.currentTimeMillis() - startTime < 60*10*1000) {
		      currentStatus = getCombServiceStatus(microserviceId)
		          if (currentStatus == "creating"){
				      println("##The new service is creating !!")
                              	      sleep 10
				      
		          } else {
				      println("##The service is ready")
				      assert currentStatus == "create_succ"
                              	      flag = true				
				 }
		       }
	    	     if (false == flag)
   		     println("##Service is timeout")
		} catch (Exception e) {
			println(e)
			flag = false
		} 	        
       return flag
    }
