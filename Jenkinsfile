pipeline 
{
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
			UIPATH_ORCH_APP_SCOPE = "OR.Administration OR.Analytics OR.Assets OR.BackgroundTasks OR.Execution OR.Folders OR.Jobs OR.Machines OR.Monitoring OR.Robots OR.Tasks OR.Queues OR.Settings OR.TestDataQueues OR.TestSetExecutions OR.TestSets OR.TestSetSchedules OR.Users OR.Webhooks"
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
                      outputPath: "${WORKSPACE}\\Output\\${env.BUILD_NUMBER}",
					  outputType: 'Tests',
                      projectJsonPath: "project.json",
                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
                      useOrchestrator: false,
					  traceLevel: 'Verbose'
					)
	            }
	        }
			
	         // Deploy Stages
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "               
				    UiPathDeploy (
						packagePath: "${WORKSPACE}\\Output\\${env.BUILD_NUMBER}", 
						orchestratorAddress: "${UIPATH_ORCH_URL}", 
						orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
						folderName: "${UIPATH_ORCH_FOLDER_NAME}",
						environments: '',
						createProcess: true,
						//credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
						//credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey'],
						credentials: ExternalApp(accountForApp: "${UIPATH_ORCH_LOGICAL_NAME}", 
										applicationId: "${UIPATH_ORCH_APP_ID}", 
										applicationScope: "${UIPATH_ORCH_APP_SCOPE}", 
										applicationSecret: 'CloudOrchAppSecret', 
										identityUrl: ''),
						traceLevel: 'Verbose',
						entryPointPaths: 'Main.xaml'
					)
					
					echo "Running Tests in UAT"
					UiPathTest (
						testTarget: [$class: 'TestSetEntry', testSet: "TestSet1"],
						orchestratorAddress: "${UIPATH_ORCH_URL}",
						orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
						folderName: "${UIPATH_ORCH_FOLDER_NAME}",
						disableBuiltInNugetFeeds: false,
						traceLevel: 'Verbose',
						timeout: '10000',
						testResultsOutputPath: '',
						credentials: ExternalApp(accountForApp: "${UIPATH_ORCH_LOGICAL_NAME}", 
										applicationId: "${UIPATH_ORCH_APP_ID}", 
										applicationScope: "${UIPATH_ORCH_APP_SCOPE}", 
										applicationSecret: 'CloudOrchAppSecret', 
										identityUrl: ''),
					)
				}
	        }
			
			
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
		
	
}
