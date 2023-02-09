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


     stage('Notification') {
      steps {
        notifyEvents message: 'Successfully deployed', token: 'cDNnrGsMlftwwTnvRVoK6jVAKkLK_iYA'
        }

      }
    }


  post {
        failure {
            notifyEvents message: 'Failed deployed', token: 'cDNnrGsMlftwwTnvRVoK6jVAKkLK_iYA'

            }
      }

 }
