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
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/tmp/build_dir -e HOME=/tmp/build_dir -e DOTNET_CLI_TELEMETRY_OPTOUT=1") {
	   sh 'pwd'
	   sh 'ls -al'
	   sh 'ls -al /'
	   sh 'ls -al /tmp/build_dir'
	   sh 'cp -r * /tmp/build_dir'
	   sh 'cd /tmp/build_dir && ls -al'
       sh 'dotnet build -c Release -o /app/build'
     }
  }
  stage('test') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/tmp/build_dir") {
	   sh 'cd /tmp/build_dir'
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
