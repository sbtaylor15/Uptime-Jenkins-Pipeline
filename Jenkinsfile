#!/usr/bin/env groovy

@Library('deployhub') _

def app="ChiliUptimeApp"
def environment=""
def cmd=""
def url="https://console.deployhub.com"
def user="jmorada"
def pw="american.edu"

def dh = new deployhub();

node {
	
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
   
      echo "Moving $app from Development to Testing";
	    
      def attrs = [lastbuildnumber: "20"];
      data = dh.updateComponentAttrs(url,user,pw, "Uptime Dashboard;1",attrs);
      echo "Update Done " + data.toString();
     
       data = dh.moveApplication(url,user,pw, app ,"GLOBAL.American University.CSC589.chili.Development","Move to Testing");
       if (data[0])
       {
        echo "Deploying $app to Testing"
        
        data = dh.deployApplication(url,user,pw, app, "GLOBAL.American University.CSC589.chili.Testing.Test");
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

    stage ('Production') {
    
      echo "Moving $app from Development to Production"
     
       data = dh.moveApplication(url,user,pw, app ,"GLOBAL.American University.CSC589.chili.Testing","Move to Production");
       if (data[0])
       {
        echo "Deploying $app to Production"
        
        data = dh.deployApplication(url,user,pw, app, "GLOBAL.American University.CSC589.chili.Production.Prod");
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
