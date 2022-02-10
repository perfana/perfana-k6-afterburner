


pipeline {

   agent any

    parameters {
        string(name: 'system_under_test', defaultValue: 'BumbleBee', description: 'Name used as System Under Test in Perfana')
        string(name: 'gatlingRepo', defaultValue: 'https://github.com/perfana/perfana-k6-afterburner.git', description: 'Gatling git repository')
        choice(name: 'workload', choices: ['test-type-load', 'test-type-stress', 'test-type-slow-backend', 'test-type-cpu'], description: 'Workload profile to use in your Gatling script')
        string(name: 'annotations', defaultValue: '', description: 'Add annotations to the test run, these will be displayed in Perfana')
        string(name: 'targetBaseUrl', defaultValue: 'http://bumble-bee-fe:8080', description: 'Target Url')
        booleanParam(name: 'kubernetes', defaultValue: false, description: 'Run in Kubernetes')

    }

    stages {

        stage('Checkout') {

            steps {

                script {

                    git url: params.gatlingRepo, branch: 'master'

                }

            }

        }

        stage('Run performance test') {

            steps {

                script {

                    def testRunId = env.JOB_NAME + "-" + env.BUILD_NUMBER
                    def version = "2.0." + env.BUILD_NUMBER
                    def buildUrl = env.BUILD_URL
                    def kubernetes = (params.kubernetes == true) ? "-Pkubernetes" : ""


                    // ** NOTE: This 'M3' maven tool must be configured
                    // **       in the global configuration.

                    def mvnHome = tool 'M3'


                    withCredentials([string(credentialsId: 'perfanaApiKey', variable: 'TOKEN')]) {

                        sh """
                           ${mvnHome}/bin/mvn clean install -X -U event-scheduler:test -Ptest-env-demo,${params.workload},assert-results -DtestRunId=${testRunId} -DbuildResultsUrl=${buildUrl} -Dversion=${version} -DsystemUnderTest=${system_under_test} -Dannotations="${params.annotations}" -DapiKey=$TOKEN -DtargetBaseUrl=${targetBaseUrl} ${kubernetes}
                        """
                    }


                }
            }
        }
    }

}
