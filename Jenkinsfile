pipeline {
    agent any
    stages {
	
        stage('Create Packages') {
		steps {
       build job: 'Create Packages', parameters: [[$class: 'StringParameterValue', name: 'componentIds', value: componentIds], [$class: 'StringParameterValue', name: 'processNames', value: processNames], [$class: 'StringParameterValue', name: 'notes', value: notes], [$class: 'StringParameterValue', name: 'packageVersion', value: packageVersion], [$class: 'StringParameterValue', name: 'listenerStatus', value: 'RUNNING'],[$class: 'StringParameterValue', name: 'extractComponentXmlFolder', value: extractComponentXmlFolder],[$class: 'StringParameterValue', name: 'tag', value: tag],[$class: 'StringParameterValue', name: 'componentType', value: componentType]]
    }
	}
    stage('Deploy to Dev') {
	steps {
       build job: 'Deploy Packages', parameters: [[$class: 'StringParameterValue', name: 'componentIds', value: componentIds], [$class: 'StringParameterValue', name: 'processNames', value: processNames], [$class: 'StringParameterValue', name: 'env', value: InitialEnvironment], [$class: 'StringParameterValue', name: 'notes', value: notes], [$class: 'StringParameterValue', name: 'packageVersion', value: packageVersion], [$class: 'StringParameterValue', name: 'listenerStatus', value: 'RUNNING'],[$class: 'StringParameterValue', name: 'extractComponentXmlFolder', value: extractComponentXmlFolder],[$class: 'StringParameterValue', name: 'tag', value: tag],[$class: 'StringParameterValue', name: 'componentType', value: componentType]]
    }   
}	
    stage('Deploy Test Process') {
	steps {
        build job: 'Deploy Packages', parameters: [[$class: 'StringParameterValue', name: 'componentIds', value: TestProcessId], [$class: 'StringParameterValue', name: 'processNames', value: TestProcessName], [$class: 'StringParameterValue', name: 'env', value: InitialEnvironment], [$class: 'StringParameterValue', name: 'notes', value: notes], [$class: 'StringParameterValue', name: 'packageVersion', value: packageVersion]]            
    }}
    stage('Execute Test Process') {
	steps {
        build job: 'Execute Process', parameters: [[$class: 'StringParameterValue', name: 'processName', value: TestProcessName], [$class: 'StringParameterValue', name: 'componentId', value: TestProcessId], [$class: 'StringParameterValue', name: 'atomName', value: atomName], [$class: 'StringParameterValue', name: 'atomType', value: '*']]                
    }
	}
    stage('Validate Test Process') {
	steps {
        build job: 'Query Execution Record', parameters: [[$class: 'StringParameterValue', name: 'processName', value: TestProcessName], [$class: 'StringParameterValue', name: 'componentId', value: TestProcessId], [$class: 'StringParameterValue', name: 'env', value: InitialEnvironment], [$class: 'StringParameterValue', name: 'atomName', value: atomName], [$class: 'StringParameterValue', name: 'checkStatus', value: 'true']]                
    } 
	}
    stage('Code Approval') {
	steps {
            
            
                    
                    emailext body: '''
                    Please go to the console output ${BUILD_URL} of the input  to approve or reject.<br>
                    ''',
                        mimeType: 'text/html',
                        subject: "[Jenkins]  Build Approval Request",
                        from: "vishnu.r@techygeekhub.com",
                        to: "vishnu.rammohankb@gmail.com",
                        recipientProviders: [[$class: 'CulpritsRecipientProvider']]

                    timeout(time: 7, unit: 'DAYS') {
                        input message: 'Do you Approve', submitter: 'admin'
                    }
                
            
        }}
    stage('Code Promotion') {
	steps {
       build job: 'Deploy Packages', parameters: [[$class: 'StringParameterValue', name: 'componentIds', value: componentIds], [$class: 'StringParameterValue', name: 'processNames', value: processNames], [$class: 'StringParameterValue', name: 'env', value: PromotionEnvironment], [$class: 'StringParameterValue', name: 'notes', value: notes], [$class: 'StringParameterValue', name: 'packageVersion', value: packageVersion], [$class: 'StringParameterValue', name: 'listenerStatus', value: listenerStatus],[$class: 'StringParameterValue', name: 'extractComponentXmlFolder', value: ''],[$class: 'StringParameterValue', name: 'tag', value: ''],[$class: 'StringParameterValue', name: 'componentType', value: componentType]]
    }
    }
    }
	 
    }
