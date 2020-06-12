node {
   
   def mvnHome
   stage('Preparation') {
      // Get some code from a GitHub repository
      build 'fsd-clone-git'
      // Get the Maven tool.
      // ** NOTE: This 'maven' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -B -f "$WORKSPACE/../fsd-clone-git/fsd-back-end/fsd-common/pom.xml" -Dmaven.test.skip=true clean package'
            sh 'cd "$WORKSPACE/../fsd-clone-git/fsd-back-end/fsd-eureka" & "$MVN_HOME/bin/mvn" -Dmaven.test.skip=true clean package docker:build'
            sh 'cd "$WORKSPACE/../fsd-clone-git/fsd-back-end/fsd-auth" & "$MVN_HOME/bin/mvn" -Dmaven.test.skip=true clean package docker:build'
         } else {
            bat label: '', script: '''"%MVN_HOME%\\bin\\mvn" -B -f "%WORKSPACE%\\..\\fsd-clone-git\\fsd-back-end\\fsd-common\\pom.xml" -Dmaven.test.skip=true clean package'''
            
            bat label: '', script: '''cd /d "%WORKSPACE%\\..\\fsd-clone-git\\fsd-back-end\\fsd-eureka"
            "%MVN_HOME%\\bin\\mvn" -Dmaven.test.skip=true clean package docker:build'''
            
            bat label: '', script: '''cd /d "%WORKSPACE%\\..\\fsd-clone-git\\fsd-back-end\\fsd-auth"
            "%MVN_HOME%\\bin\\mvn" -Dmaven.test.skip=true clean package docker:build'''
         }
      }
   }
   stage('Deploy') {
      bat label: '', script: '''
for /f "usebackq tokens=1" %%i in (`docker container ls ^| findstr eureka`) do (set id=%%i)
if not "%id%" == "" docker container kill %id%
for /f "usebackq tokens=1" %%i in (`docker container ls -a ^| findstr eureka`) do (set id=%%i)
if not "%id%" == "" docker container rm %id%
docker run -d -p 8761:8761 --name fsd-eureka waitswallow/fsd-eureka

for /f "usebackq tokens=1" %%i in (`docker container ls ^| findstr auth`) do (set id=%%i)
if not "%id%" == "" docker container kill %id%
for /f "usebackq tokens=1" %%i in (`docker container ls -a ^| findstr auth`) do (set id=%%i)
if not "%id%" == "" docker container rm %id%
docker run -d -p 2105:2105 --name fsd-auth waitswallow/fsd-auth'''
   }
}
