pipeline {
 agent any

    	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://staging.uipath.com"
	        UIPATH_ORCH_LOGICAL_NAME = "jeet"
	        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
	        UIPATH_ORCH_FOLDER_NAME = "Folder1"
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
					  traceLevel: 'None'
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
                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: '5b39e97b-4985-42b8-a060-235825a290aa'), 
				credentials: ExternalApp(accountForApp: 'jeet', applicationId: '992484da-6b8e-412d-8625-472158a5e5f0', applicationScope: 'OR.Administration OR.Administration.Read OR.Administration.Write OR.Analytics OR.Analytics.Read OR.Analytics.Write OR.Assets OR.Assets.Read OR.Assets.Write OR.Audit OR.Audit.Read OR.Audit.Write OR.BackgroundTasks OR.BackgroundTasks.Read OR.BackgroundTasks.Write OR.Execution OR.Execution.Read OR.Execution.Write OR.Folders OR.Folders.Read OR.Folders.Write OR.Hypervisor OR.Hypervisor.Read OR.Hypervisor.Write OR.Jobs OR.Jobs.Read OR.Jobs.Write OR.License OR.License.Read OR.License.Write OR.Machines OR.Machines.Read OR.Machines.Write OR.ML OR.ML.Read OR.ML.Write OR.Monitoring OR.Monitoring.Read OR.Monitoring.Write OR.Queues OR.Queues.Read OR.Queues.Write OR.Robots OR.Robots.Read OR.Robots.Write OR.Settings OR.Settings.Read OR.Settings.Write OR.Tasks OR.Tasks.Read OR.Tasks.Write OR.TestDataQueues OR.TestDataQueues.Read OR.TestDataQueues.Write OR.TestSetExecutions OR.TestSetExecutions.Read OR.TestSetExecutions.Write OR.TestSets OR.TestSets.Read OR.TestSets.Write OR.TestSetSchedules OR.TestSetSchedules.Read OR.TestSetSchedules.Write OR.Users OR.Users.Read OR.Users.Write OR.Webhooks OR.Webhooks.Read OR.Webhooks.Write', applicationSecret: '%m**vYDXvF7gzu5l', identityUrl: ''),
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
