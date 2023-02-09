pipeline {
  agent any
  stages {
  stage('Test'){
              steps {
                  bat './gradlew test'

              }

          }
          stage('sonar') {
            steps {
            withSonarQubeEnv('SonarQube') {
                                bat "./gradlew sonarqube"
                            }

            }
          }

          stage("Quality Gate") {
                      steps {
                          timeout(time: 1, unit: 'HOURS') {
                              waitForQualityGate abortPipeline: true
                          }
                      }
                  }
          stage('build'){
                        steps {
                            bat './gradlew build'
                            bat './gradlew jar'
                            bat './gradlew javadoc'
                            archiveArtifacts 'build/libs/*.jar'

                        }

                    }
                    stage('deploy') {
                    steps {
                     bat './gradlew publish'
                    }
                    post{
                                                always{
                                                    slackSend(  message: "Ahmed new build !!!!",)
                                                    mail to: "if_benmoumen@esi.dz",
                                                    subject: "New build !!",
                                                    body: "Jenkins new build !!"
                                                }
                                            }

                    }
}
post {
      always {
        junit skipPublishingChecks: true, testResults: 'build/test-results/test/TEST-Matrix.xml'
        archiveArtifacts 'build/test-results/test/TEST-Matrix.xml'
      }
      failure {  mail to: "if_benmoumen@esi.dz",
                 subject: "Failure Email",
                 body: "Build failed"
      }
   }
}