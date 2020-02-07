import jenkins.model.*
pipeline {
	agent {
			label 'master'
			}

	options { 
		disableConcurrentBuilds() 
		buildDiscarder(logRotator(numToKeepStr: '10'))
		timestamps()
		parallelsAlwaysFailFast()
		}
		
	parameters {
	  string defaultValue: 'branch', description: 'input your branch', name: 'branch', trim: true
	  extendedChoice defaultValue: 'ser1@1,ser2@2,ser3@3,ser4@4', description: 'select a service', descriptionPropertyValue: 'ser1@1,ser2@2,ser3@3,ser4@4', multiSelectDelimiter: ',', name: 'project_name', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_CHECKBOX', value: 'ser1@1,ser2@2,ser3@3,ser4@4', visibleItemCount: 4
	}
	
	stages {

		//stage('matrix'){
			//matrix {
				//axes {
					//axis {
						//name 'PLATFORM'
						//values 'master'
					//}
					//axis {
						//name 'BROWSER'
						//values 'master'
					//}
				//}
				//stages {
					//stage('build-and-test') {
						//steps{println("hello world")}
					//}
				//}
			//}
		 //}//
		 
		 stage('checkout') {
		 steps {
			 script{				
					 checkout([$class: 'GitSCM', 
						 branches: [[name: '*/master']], 
						 doGenerateSubmoduleConfigurations: false, 
						 //extensions: [[$class: 'PerBuildTag']], 
						 submoduleCfg: [], 
						 userRemoteConfigs: [[credentialsId: 'git_ssh', 
						 url: 'git@github.com:xiangliangliang/pipelin_parallel_list.git']]])
					 }

			 }
			}//

		stage('add tag') {
			steps {
				script{	
						def d=new Date().toString().split()[3].replaceAll(":","_")
						tag = "${d}_tag"
						git credentialsId: 'github_test', url:"git@github.com:xiangliangliang/pipelin_parallel_list.git"
						bat """	
							cd D:/git_pipeline_parallel
							git config user.email "123@qq.com"
							git config user.name "123@qq.com"
							git tag -a -m "${tag}" ${tag}
							git push origin --tags
						"""

						}

				}
			}//

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

					for (x in select_pro) {
						def pro = x //特别注意，这里必须再转换一下，it is a must transform, otherwise the result will be ser4@4 ser4@4 ser4@4 ser4@4
						   stage ("${pro}"){ 
								branches["${pro}"] = { 
									node ('master'){
										println("${pro.split('@')[0]}：${pro.split('@')[1]} ")
										sleep 5
										def d=new Date().toString() 
										println(d)

									}
								}
						  }
					}
					parallel branches
				}
			}
		}//
		
	}
}
