#!/usr/bin/env groovy

@Library('deployhub') _

def app="ChiliUptimeApp"
def environment=""
def cmd=""
def url="http://192.168.3.96:7171"
def user="dhuser"
def pw="dhuser"

def dh = new deployhub();

node {
	
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
   
def comp="GLOBAL.Test_Project.Test.testapp"
def version = "0.1.0-103"
def imagename = "app-ui-helm"

echo "${url}";
echo "${version}";

// create component version
// def newComponentVersion(String url, String userid, String pw, String compname, String compvariant, String compversion)
data = dh.newComponentVersion(url, user, pw, comp, "", version);
echo "Creation Done " + data.toString();

// // update attrs
def attrs = [
	     BuildId: env.BUILD_ID,
	     BuildUrl: - env.BUILD_URL,
             Chart: "harbor-lib/"+imagename, 
	     chartversion: version,
	     operator: "Kubernetes operator",
	     DockerBuildDate: "timestamp",
	     DockerSha: "sha for the docker image",
	     DockerRepo: "url for the docker registry",
	     GitCommit: env.GIT_COMMIT,
	     GitRepo: env.GIT_URL,
	     GitTag: env.GIT_BRANCH,
	     GitUrl: env.GIT_URL,
	     buildnumber: env.BUILD_ID, 
	     buildjob: env.JOB_NAME,
	     ComponentType: "Application File",
	     ChangeRequestDS: "GLOBAL.JiraUnisys",
	     Category: "General",
	     AlwaysDeploy: "Y",
	     DeploySequentially: "Y",
	     BaseDirectory: "tmp",
	     PreAction: "GLOBAL.CatchRC",
	     PostAction: "GLOBAL.GetLog",
	     CustomAction: "GLOBAL.RunJenkinsJob"
	    ];
echo "${attrs}";
// // def updateComponentAttrs(String url, String userid, String pw, String compname, String compvariant, String compversion, Map Attrs)
data = dh.updateComponentAttrs(url, user, pw, comp, "", version , attrs);
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
