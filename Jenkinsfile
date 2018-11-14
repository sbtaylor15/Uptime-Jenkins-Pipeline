#!/usr/bin/env groovy

@Library('deployhub') _

def app="GLOBAL.American University.CSC589.chili.Uptime App"
def environment=""
def cmd=""
def url=""
def user="admin"
def pw="admin"

def dh = new deployhub();

node {
    
    url = dh.getURL(env);
	
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
   
      echo "Moving $app from Development to Testing"
     
       data = dh.moveApplication(url,user,pw, app ,"GLOBAL.American University.CSC589.chili.Development","Move to Testing");
       if (data[0])
       {
        echo "Deploying $app to Testing"
        
        data = dh.deployApplication(url,user,pw, app, "GLOBAL.American University.CSC589.chili.Testing");
        if (data[0])
        {
         def deploymentid = data[1]['deploymentid'];

         echo "Deployment Logs for #$deploymentid"
         data = dh.getLogs(url,user,pw, "$deploymentid");
         echo data[1];         
        }
        else
        {
         error(data[1]);
        } 
       }
       else
       {
        error(data[1]);
       } 
      }
    }  

    stage ('Production') {
    
      echo "Moving $app from Development to Production"
     
       data = dh.moveApplication(url,user,pw, app ,"GLOBAL.American University.CSC589.chili.Testing","Move to Production");
       if (data[0])
       {
        echo "Deploying $app to Production"
        
        data = dh.deployApplication(url,user,pw, app, "GLOBAL.American University.CSC589.chili.Production");
        if (data[0])
        {
         def deploymentid = data[1]['deploymentid'];

         echo "Deployment Logs for #$deploymentid"
         data = dh.getLogs(url,user,pw, "$deploymentid");
         echo data[1];         
        }
        else
        {
         error(data[1]);
        } 
       }
       else
       {
        error(data[1]);
       } 
      }
    }  

}
