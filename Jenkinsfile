#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    //def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    /* agent any
    environment {
        // Removed other variables for clarity...
        SFDX_USE_GENERIC_UNIX_KEYCHAIN = true
        // ...
    } */

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {


        stage('Authorize DevHub') {
            rc = bat "sfdx force:auth:jwt:grant --instanceurl ${SFDC_HOST} --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --setalias HubOrg"
            if (rc != 0) {
                error 'Salesforce dev hub org authorization failed.'
            }

            println rc
        }

        // stage('Deploy Code') {
        //     /* if (isUnix()) {
        //         rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile //${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
        //     }else{
        //          rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile //\"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
        //     }
        //     if (rc != 0) { error 'hub org authorization failed' }

		// 	println rc */
			
		// 	// need to pull out assigned username
		// 	if (isUnix()) {
		// 		rmsg = sh returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
		// 	}else{
		// 	   rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
		// 	}
			  
        //     printf rmsg
        //     println('Hello from a Job DSL script!')
        //     println(rmsg)
        // }
    }
}
