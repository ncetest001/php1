node('java-slave-1') {
  stage('Prepare') {
    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'gitlab',
      usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
      sh 'rm -rf java-demo'
      sh "git clone http://'${USERNAME}':'${PASSWORD}'@139.129.99.183:10080/root/java-demo.git"     
    }
  }
  stage('check image’){
…
}
  stage('upgrade service') {
    …
   }
  
  
  stage('ci test') {
    …
  }
 }
