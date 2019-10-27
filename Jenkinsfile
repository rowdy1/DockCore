node {
  def server = Artifactory.server 'artifactory'
  def myDotNetSDKContainer = docker.image('rowdy1/dotnet:3.1')
  myDotNetSDKContainer.pull()
  
  stage('prep') {
    checkout scm
	sh 'ls -al'
	sh 'groups'
  }
  stage('build') {
     myDotNetSDKContainer.inside("-v ${env.WORKSPACE}:/app -e HOME=/app -e DOTNET_CLI_TELEMETRY_OPTOUT=1") {
	   sh 'pwd'
       sh 'dotnet build -c Release -o /app/build'
     }
  }
  stage('test') {
     myDotNetSDKContainer.inside("-v ${env.WORKSPACE}:/app -e HOME=/app -e DOTNET_CLI_TELEMETRY_OPTOUT=1") {
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
