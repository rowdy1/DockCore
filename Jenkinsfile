node {
  def server = Artifactory.server 'artifactory'
  def myDotNetSDKContainer = docker.image('mcr.microsoft.com/dotnet/core/sdk:3.1-buster')
  myDotNetSDKContainer.pull()
  stage('prep') {
    checkout scm
  }
  stage('build') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/tmp -e HOME=/tmp -e DOTNET_CLI_TELEMETRY_OPTOUT=1") {
	   sh 'pwd'
	   sh 'ls -al'
	   sh 'ls -al /'
	   sh 'ls -al /tmp'
	   sh 'cp -r * /tmp'
	   sh 'cd /tmp'
	   sh 'ls -al'
	   sh 'dotnet test'
     }
  }
  stage('test') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/src/.dotnet") {
       sh 'ls && ./dotnet test WorkerL'
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
