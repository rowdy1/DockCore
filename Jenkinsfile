node {
  def server = Artifactory.server 'artifactory'
  def myDotNetSDKContainer = docker.image('mcr.microsoft.com/dotnet/core/sdk:3.1-buster')
  myDotNetSDKContainer.pull()
  stage('prep') {
    checkout scm
  }
  stage('build') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/tmp/dotnet/.dotnet -e HOME=/tmp -e DOTNET_CLI_TELEMETRY_OPTOUT=1") {
	   sh 'pwd'
	   sh 'ls -al'
	   sh 'ls -al /tmp/dotnet'
	   sh 'ls -al /'
	   sh 'cd ..'
	   sh 'pwd'
	   sh 'cp -r /var/jenkins_home/workspace/dotnet /tmp'
	   sh 'cd /tmp'
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
