node {
  def server = Artifactory.server 'artifactory'
  def myDotNetSDKContainer = docker.image('mcr.microsoft.com/dotnet/core/sdk:3.1-buster')
  myDotNetSDKContainer.pull()
  stage('prep') {
    checkout scm
  }
  stage('build') {
     myDotNetSDKContainer.inside("-v ${env.HOME}:/src") {
       sh 'ls && ./dotnet restore && dotnet build -c Debug -o /app/Debug'
     }
  }
  stage('test') {
     myDotNetSDKContainer.inside("-v ${env.HOME}:/src") {
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
