node {
  def server = Artifactory.server 'artifactory'
  def myDotNetSDKContainer = docker.image('rowdy1/dotnet:3.1')
  
  stage('prep') {
    checkout scm
	sh 'ls -al'
	sh 'groups'
  }
  myDotNetSDKContainer.pull()
  
  stage('build') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/tmp/src -e HOME=/tmp/src -e DOTNET_CLI_TELEMETRY_OPTOUT=1") {
	   sh 'pwd'
	   sh 'ls -al'
	   sh 'ls -al /'
	   sh 'ls -al /tmp/src'
	   sh 'cp -r * /tmp/src'
	   sh 'cd /tmp/src && ls -al'
       sh 'dotnet build -c Release -o /tmp/src/build'
     }
  }
  stage('test') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/tmp/src") {
	   sh 'cd /tmp/src'
       sh 'dotnet test'
     }
  }
  stage('publish') {
    def uploadSpec = """{
      "files": [
        {
          "pattern": "complete/build/libs/gs-gradle-*.jar",
          "target": "gradle-dev-local/rowdy1/gs-gradle/1.0/gs-gradle-1.0-docker.jar"
        }
     ]
    }"""
    server.upload(uploadSpec)
  }
}
