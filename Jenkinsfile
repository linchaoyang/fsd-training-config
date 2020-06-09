node {
   
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      build 'fsd-clone-git'
   }
   stage('Build') {
      // Run the maven build
      build 'build-fsd-common'
      build 'build-fsd-eureka'
      build 'build-fsd-auth'
   }
   stage('Deploy') {
      bat label: '', script: '''set id=
for /f "usebackq tokens=1" %i in (`docker container ls ^| findstr eureka`) do (set id=%i)
if not "%id%" == "" docker container kill %id%
docker run -d -p 8761:8761 --name fsd-eureka waitswallow/fsd-eureka

set id=
for /f "usebackq tokens=1" %i in (`docker container ls ^| findstr auth`) do (set id=%i)
if not "%id%" == "" docker container kill %id%
docker run -d -p 2105:2105 --name fsd-auth waitswallow/fsd-auth'''
   }
}
