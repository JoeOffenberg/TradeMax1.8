pipeline {
    agent any
    stages {
         stage('Retrieve Sources') {
              steps {
	 snDevOpsStep(enabled:true)	      
         echo workspace
         git branch: 'master',
    	     credentialsId: '16753248-fe59-444b-b9b6-f91f07da7944',
             url: 'https://github.com/JoeOffenberg/TradeMax1.8.git'
         
    	}
    }
      stage ('Validation'){
                                 
        parallel {
        
        stage ('Config'){
		stages ('Sweagle Steps'){
		           
		       

        stage('UploadConfig'){
        
            steps {
                snDevOpsStep(enabled:true)
                SWEAGLEUpload(
                actionName: 'Upload Config Files', 
                fileLocation: "WebContent/META-INF/my.cnf", 
                format: 'INI', 
                markFailed: false, 
                nodePath: 'Infrastructure,db023,mycnf', 
                onlyParent: false, 
                showResults: false,
                withSnapshot: false,
                description: 'Upload MySQL config',
                tag: '', 
                autoRecognize: true,
                allowDelete: false)
                
                SWEAGLEUpload(
                actionName: 'Upload JSON Files', 
                fileLocation: "*.json", 
                format: 'json', 
                markFailed: false, 
                nodePath: 'Applications,TradeMax,Discovered,Files', 
                onlyParent: false, 
                showResults: false,
                withSnapshot: false,
                subDirectories: true,
                description: 'Upload json files',
                tag: '', 
                autoRecognize: false,
                allowDelete: false)

            }
        }
        
            stage('Validate Config') {
                steps {
		    snDevOpsStep(enabled:true)	
                    SWEAGLEValidate(
                    actionName: 'Validate Config Files',
                    mdsName: 'TradeMax-PRD',
                    stored: false,
                    warnMax: -1,
                    errMax: 0,
                    markFailed: true,
                    showResults: false, 
                    retryCount: 5,
                    retryInterval: 30)
                    }
            	}	
            
            
       
                
            
        stage('Snapshot Config') {
            steps {
	      snDevOpsStep(enabled:true)	    
              SWEAGLESnapshot(
              actionName: 'Validated Snapshot TradeMax-PRD',
              mdsName: 'TradeMax-PRD',
              description: "Validated Snapshot for Jenkins Build ${BUILD_ID}",
              tag: "Version:1.8.${BUILD_ID}",
              markFailed: false,
              showResults: false)
              
              
            }
        }
        
        stage('Export Config') {
            steps {
	      snDevOpsStep(enabled:true)	    
              SWEAGLEExport(
              actionName: 'Export TradeMax-PRD settings.json',
              mdsName: 'TradeMax-PRD',
              exporter: 'returnDataforNode',
              args: "mycnf",
              format: 'json',
              fileLocation: "settings.json",
              markFailed: true,
              showResults: true)
              
              
            }
        }
			}
			} //Sweagle versioining and validation
			
		stage ('Code'){ 
		stages{
    			
			    stage('jUnit Test'){ 
                steps { snDevOpsStep(enabled:true)
			echo "Testing..."
                     }
                  }
                  
                stage('SonarQube'){ 
                steps { snDevOpsStep(enabled:true)
			sh 'sonar-scanner 55'
                     }
                  }
                  
                  }	
                 
                  
        }
       } //parallel
    } //Validation Stage
    
    
    stage ('Build'){
    steps { snDevOpsStep(enabled:true)
	    sleep(time:35,unit:"SECONDS")
                     }
    
    }
    
    stage ('Deployment'){
	    
    steps { snDevOpsStep(enabled:true)
	    snDevOpsChange()
	    sleep(time:35,unit:"SECONDS")
                     }
    
    }
    
    stage (Functional) {
        parallel {
       	          			
			    stage('Selenium API'){ 
                steps { snDevOpsStep(enabled:true)
			echo "Selenium API..2..3..4"
                		sleep(time:25,unit:"SECONDS")
                		echo "Selenium API..2..3..4"
                     }
                  }
                  
                stage('Selenium UI'){ 
                steps {	snDevOpsStep(enabled:true)
			echo "Selenium UI..2..3..4"
                		sh 'selenium 35'
                		echo "Selenium API..2..3..4"
                     }
                  }
                 
    }
    
    }//Functional Testing
    
 } //Outer Stages
 post {
        always {
            junit 'sweagle-validation.xml'
        }
    }
} //Pipeline
