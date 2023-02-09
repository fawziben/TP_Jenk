pipeline {
  agent any
  stages {

   stage ('Test') { // la phase build
    steps {
    bat 'gradlew test'
     junit 'build/test-results/test/TEST-Matrix.xml'

       cucumber buildStatus: 'UNSTABLE',
                    reportTitle: 'My report',
                    fileIncludePattern: 'target/report.json',

                    trendsLimit: 10

    }
}




        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradlew sonar'
            }


          }
        }


         stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }



     stage('build') {


      steps {
        bat(script: 'gradlew build', label: 'gradle build')
        bat 'gradlew javadoc'
        archiveArtifacts 'build/libs/*.jar'


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
