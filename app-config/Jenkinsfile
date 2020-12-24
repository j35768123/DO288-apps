/*
 * Copyright © 2010 - 2013 Apama Ltd.
 * Copyright © 2013 - 2018 Software AG, Darmstadt, Germany and/or its licensors
 *
 * SPDX-License-Identifier: Apache-2.0
 *
 *   Licensed under the Apache License, Version 2.0 (the "License");
 *   you may not use this file except in compliance with the License.
 *   You may obtain a copy of the License at
 *
 *       http://www.apache.org/licenses/LICENSE-2.0
 *
 *   Unless required by applicable law or agreed to in writing, software
 *   distributed under the License is distributed on an "AS IS" BASIS,
 *   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *   See the License for the specific language governing permissions and
 *   limitations under the License.                                                            
 *
 */
 
def skipRemainingStages = false
  
pipeline {

	agent any

	stages {
	
		stage('Prepare') {
 
			steps {
 
				script{
				    
				    def status= 0

                    if (status != 0) {
	                    throw new Exception("sh command failed")
                    }
				    
				     serverType="${SERVER_TYPE}"
				     packageType = ""
				   
		             if (serverType == "RTDev") {
				        packageType = "/RT/Packages"
				     } else {
				        packageType = "/BT/Packages"
				     }
				    
				    println("The packageType = " + packageType)	
				    
				    def config = readFile "project.properties"

                    def newconfig = config.replaceAll("isPackageToBuild=.*","isPackageToBuild=${PACKAGE_NAME}").replaceAll("isPackages=.*","isPackages=${packageType}")

                    writeFile file: "project.properties", text: "${newconfig}"

					echo 'Prepare..'
				}
			
			}
		}
		
		stage('Multiple Target host'){
 
			steps {
 
				script{
					echo 'Multiple Target host..'
				}
			}
		}
		
		stage('Build'){
 
			steps {
				echo 'Build..'
			
				script{   	
			        String serverType = "${SERVER_TYPE}";
			        println(serverType)
			        
			        println("/appl/jenkins/workspace/EI-WAM-WM-Cook-Dev_Deployment/"+ serverType.substring(0, 2) + "/Packages")
			        
                    if(serverType == "RTDev"){
                        println(serverType)
                    }else if(serverType == "BTDev") {
                        println(serverType)
                    }
			    }
				
            }
		}
		
		stage('Deploy-1') {
 
            steps {              
 
				script{    
				    skipRemainingStages = false
				    
				     try {
                        println("In if block")
                        skipRemainingStages = false
                    }
                    catch (Exception exc) {
                        println("Exception block: ${exc}")
                        skipRemainingStages = true
                    }

                    if (skipRemainingStages) {
                        currentBuild.result = 'FAILURE'
                        error("Stopping early!")
                    }
				    
					echo 'Deploy..'
				}
			} 
        }
		
        stage('Deploy-2') {
 
            steps {
				echo 'Deploy..'
            }
        }
		
		stage('Test') {
 
            steps {
				echo 'Test..'
            }
        }
		
		stage('Zip all packages'){
 
			steps {               
 
				script{             
					echo 'Zip all packages..'
				}
			}
        }
		
		stage('Push to Artifactory'){
 
            steps {
 
                script {
					echo 'Push to Artifactory..'
				}
            }
        }
		
		
		stage ('Promotion') {
 
            steps {
				echo 'Promotion..'
            }
        }

	}
	
	post {
        always {
            emailext body: '$PROJECT_NAME - BUILD # $BUILD_NUMBER - $BUILD_STATUS:  Check console output at $BUILD_URL to view results.', 
            subject: '$PROJECT_NAME - BUILD # $BUILD_NUMBER - $BUILD_STATUS!',
            to: 'cjpalmer@us.ibm.com; cjpalmer@aep.com'
        }
        success {
            echo 'I succeeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
    }
	
}