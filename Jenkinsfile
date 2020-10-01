pipeline {
   agent{
       label 'master'
   }

   stages {
      stage('Run Jmeter Test') {
         steps {
           bat 'D:/JMETER/apache-jmeter-4.0/bin/jmeter.bat -n -t D:/Jenkins_Load_Tests/ISH_Test/ISH_Mixed_NewPaymentFlow_BenchLow.jmx -l D:/Jenkins_Load_Tests/ISH_Test/BenchLow_results_round1.jtl'
           sleep time: 1, unit: 'MINUTES'
           bat 'D:/JMETER/apache-jmeter-4.0/bin/jmeter.bat -n -t D:/Jenkins_Load_Tests/ISH_Test/ISH_Mixed_NewPaymentFlow_BenchLow.jmx -l D:/Jenkins_Load_Tests/ISH_Test/BenchLow_results_round2.jtl'
         }
      }
      stage('Publish performance results') {
         steps {
           perfReport compareBuildPrevious: true, errorFailedThreshold: 1, errorUnstableThreshold: 1, filterRegex: '', sourceDataFiles: 'D:/Jenkins_Load_Tests/ISH_Test/*.jtl'
          }
         }
      }
          environment {
        EMAIL_TO = 'manjunath.ramareddy@tomtom.com,manjunath.ramareddy@mindtree.com'
    }
    post {
        success {
            emailext body: 'I will always say Hello again! \n\n Build success in Jenkins \n\n Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build success in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
		failure {
            emailext body: 'I will always say Hello again! \n\n Build failed in Jenkins \n\n Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
    }
}
