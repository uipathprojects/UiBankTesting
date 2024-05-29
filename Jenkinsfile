pipeline {
 agent any

    	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "jeetriauqow"
	        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
	        UIPATH_ORCH_FOLDER_NAME = "Shared"
			UIPATH_ORCH_APP_ID = "545b4acc-615e-477f-9723-d61a451e344a"
			UIPATH_ORCH_APP_SECRET = "7kKn)6XpiERYq5Z4"
			UIPATH_ORCH_APP_SCOPE = "OR.Administration OR.Analytics OR.Assets OR.Audit OR.BackgroundTasks OR.Execution OR.Folders OR.Hypervisor OR.Jobs OR.License OR.Machines OR.ML OR.Monitoring OR.Robots OR.Tasks OR.Queues OR.Settings OR.TestDataQueues OR.TestSetExecutions OR.TestSets OR.TestSetSchedules OR.Users OR.Webhooks"
			UIPATH_ORCH_API_USERKEY = "1Fj8lfnYsua2gvOwwWJUK4qKfDgerAQVaSUnumt0fzJPx"
		}
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
                      outputPath: "Output\\${env.BUILD_NUMBER}",
					  outputType: 'Tests',
                      projectJsonPath: "project.json",
                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
                      useOrchestrator: false,
					  traceLevel: 'Verbose'
					)
	            }
	        }
			
			
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
	

	         // Deploy Stages
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
                UiPathDeploy (
                packagePath: "Output\\${env.BUILD_NUMBER}",
                orchestratorAddress: "${UIPATH_ORCH_URL}",
                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
				environments: '',
				createProcess: true,
                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: "${UIPATH_ORCH_API_USERKEY}"), 
				credentials: ExternalApp(
				accountForApp: "${UIPATH_ORCH_LOGICAL_NAME}", 
				applicationId: "${UIPATH_ORCH_APP_ID}", 
				applicationScope: "${UIPATH_ORCH_APP_SCOPE}", 
				applicationSecret: "${UIPATH_ORCH_APP_SECRET}", 
				identityUrl: "${UIPATH_ORCH_URL}"),
				traceLevel: 'Verbose',
				entryPointPaths: 'Main.xaml'
	        )
	        }
	        }
	

	

	         // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	

	}
