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
      bat script: '''docker run -d -p 8761:8761 waitswallow/fsd-eureka
        docker run -d -p 2105:2105 waitswallow/fsd-auth'''
   }
}
