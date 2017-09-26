#!/usr/bin/env groovy

def app=""
def env=""

node {
    
    stage('Clone sources') {
        git url: 'https://github.com/OpenMake-Software/Uptime-Application.git'
    }
				
    stage ('Integration') {
				  def lines=readFile('Deployfile').trim().split("\n");
						app=lines[1].split(':')[1].trim()
						env=lines[2].split(':')[1].trim()						
						
      echo "**********************************************************************************"    
      echo "* Deploying $app to Integration"
      echo "**********************************************************************************"
						def cmd = /dhdeploy.py --app "${app}" --env "Uptime Integration"/
      sh cmd
      
      echo "**********************************************************************************"
      echo "* Running Testcases for $app in Integration"
      echo "**********************************************************************************"
      sh "/usr/local/bin/runtestcases.py --app \\\"$app\\\" --stage Integration"
				}		
    
    stage ('Testing') {
      echo "**********************************************************************************"    
      echo "* Moving $app from Integration to Testing"
      echo "**********************************************************************************"
      sh "/usr/local/bin/dhmove.py --app \\\"$app\\\" --from Development --to Testing" 				    
        
      echo "**********************************************************************************"        
      echo "* Deploying $app to Testing"
      echo "**********************************************************************************"
      sh "/usr/local/bin/dhdeploy.py --app \\\"$app\\\" --env \\\"Uptime Testing\\\""	

      echo "**********************************************************************************"    
      echo "* Running Testcases for $app in Testing"
      echo "**********************************************************************************"    
      sh "/usr/local/bin/runtestcases.py --app \\\"$app\\\" --stage Testing"
    }
				
    stage ('Production') {
      echo "**********************************************************************************"    
      echo "* Moving $app from Testing to Production"
      echo "**********************************************************************************"    
      sh "/usr/local/bin/dhmove.py --app \\\"$app\\\" --from Testing --to Production" 	

      echo "**********************************************************************************"        
      echo "* Deploying $app to Production"
      echo "**********************************************************************************"
      sh "/usr/local/bin/dhdeploy.py --app \\\"$app\\\" --stage \\\"Uptime Production\\\""				
    }
}
