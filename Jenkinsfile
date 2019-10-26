node {
  def server = Artifactory.server 'artifactory'
  def myDotNetSDKContainer = docker.image('mcr.microsoft.com/dotnet/core/sdk:3.1-buster').withRun('-e HOME=/tmp -e DOTNET_CLI_HOME=/tmp/DOTNET_CLI_HOME')
  myDotNetSDKContainer.pull()
  stage('prep') {
    checkout scm
  }
  stage('build') {
     myDotNetSDKContainer.inside("-v ${env.HOME}/.dotnet:/src/.dotnet") {
       sh 'whoami && pwd && ls && cd Worker.Lib && dotnet restore && dotnet build -c Debug -o /app/Debug'
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
