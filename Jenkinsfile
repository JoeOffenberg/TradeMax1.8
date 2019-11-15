pipeline {
    agent any
    stages {
         stage('Retrieve Sources') {
              steps {
         echo workspace
         git url: 'https://github.com/JoeOffenberg/TradeMax.git'
         sh "ls -la"
    	}
    }
      stage ('Validation'){
                                 
        parallel {
        
        stage ('Config'){
		stages ('Sweagle Steps'){
		           
		       

        stage('UploadConfig'){
        
            steps {
                
                SWEAGLEUpload(
                actionName: 'Upload Config Files', 
                fileLocation: "WebContent/META-INF/my.cnf", 
                format: 'INI', 
                markFailed: false, 
                nodePath: 'infra,db023,mycnf', 
                onlyParent: false, 
                showResults: true,
                withSnapshot: false,
                description: 'Upload MySQL config',
                tag: '', 
                autoRecognize: true,
                allowDelete: false)

            }
        }
        
            stage('Validate Config') {
                steps {
                    SWEAGLEValidate(
                    actionName: 'Validate Config Files',
                    mdsName: 'TradeMax-PRD',
                    warnMax: -1,
                    errMax: 0,
                    markFailed: true,
                    showResults: true, 
                    retryCount: 5,
                    retryInterval: 30)
                    }
            	}	
            
            
       
                
            
        stage('Snapshot Config') {
            steps {
              SWEAGLESnapshot(
              actionName: 'Validated Snapshot TradeMax-PRD',
              mdsName: 'TradeMax-PRD',
              description: "Validated Snapshot for Jenkins Build ${BUILD_ID}",
              tag: "Version:1.4.${BUILD_ID}",
              markFailed: false,
              showResults: false)
              
              
            }
        }
        
        stage('Export Config') {
            steps {
              SWEAGLEExport(
              actionName: 'Export TradeMax-PRD settings.json',
              mdsName: 'TradeMax-PRD',
              exporter: 'retrieveAllDataFromNode',
              args: "settings.json",
              format: 'json',
              fileLocation: "settings.json",
              markFailed: false,
              showResults: false)
              
              
            }
        }
			}
			} //Sweagle versioining and validation
			
		stage ('Code'){ 
		stages{
    			
			    stage('jUnit Test'){ 
                steps {echo "Testing..."
                     }
                  }
                  
                stage('Sonar Cube'){ 
                steps {echo "Testing..."
                     }
                  }
                  
                  }	
                 
                  
        }
       } //parallel
    } //Validation Stage
    stage ('Deployment'){
    steps {sleep(time:35,unit:"SECONDS")
                     }
    
    }
    stage ('Functional Testing'){
    stages{
    			
			    stage('Selenium API'){ 
                steps {echo "Testing..."
                     }
                  }
                  
                stage('Selenium UI'){ 
                steps {echo "Testing..."
                     }
                  }
                 
                  
                  }
    
    }//Functional Testing
    
 } //Outer Stages
} //Pipeline