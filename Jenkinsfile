import jenkins.model.*
pipeline {
	agent {
			label 'master'
			}

	options { 
		//disableConcurrentBuilds() 
		buildDiscarder(logRotator(numToKeepStr: '10'))
		timestamps()	
		}
		
	parameters {
	  string defaultValue: 'branch', description: 'input your branch', name: 'branch', trim: true
	  extendedChoice defaultValue: 'ser1@1,ser2@2,ser3@3,ser4@4', description: 'select a service', descriptionPropertyValue: 'ser1@1,ser2@2,ser3@3,ser4@4', multiSelectDelimiter: ',', name: 'project_name', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_CHECKBOX', value: 'ser1@1,ser2@2,ser3@3,ser4@4', visibleItemCount: 4
	}
	
	stages {

		stage('serial') {
				steps {
					script{				
							project_name.split(',').each{
									println("project_name is ${it.split('@')[0]} and port is ${it.split('@')[1]}")
									def d=new Date().toString() 
									println(d)
						    }

					}
				}
			}//
			
		stage('parallel') {
			steps {
				script{		
				
					def select_pro = project_name.split(',') 
					def branches = [:]

					for (pro in select_pro) {
						   stage ("${pro}"){ 
								branches["${pro}"] = { 
									//node ('master'){
										println("${pro.split('@')[0]}：${pro.split('@')[1]} ")
									//}
								}
						  }
					}
					parallel branches
				}
			}
		}//
	}
}
