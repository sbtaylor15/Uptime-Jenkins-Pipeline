#!/usr/bin/env groovy

@Library('deployhub') _

def app="ChiliUptimeApp"
def environment=""
def cmd=""
def url="http://voltron:7171"
def user="dhuser"
def pw="dhuser"

def dh = new deployhub();

node {
	
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
   
 def comp="GLOBAL.Test_Project.Test.testapp"
 def application="GLOBAL.Test_Project.Test.My Test App"	 
 def appver = "5.0"	    
 def version = "0.1.0-103"
 def imagename = "app-ui-helm"
 def String[] envs = [ "GLOBAL.Test_Project.Test.dev", "GLOBAL.Test_Project.Test.Test"]	    

 echo "${url}";
 echo "${version}";

 // create component version
 compid = dh.newComponentVersion(url, user, pw, comp, "", version);
 echo "Creation Done " + compid.toString();

 // update attrs
 def attrs = [
	     BuildId: env.BUILD_ID,
	     BuildUrl: env.BUILD_URL,
             Chart: "harbor-lib/"+imagename, 
	     chartversion: version,
	     operator: "Kubernetes operator",
	     DockerBuildDate: "timestamp",
	     DockerSha: "sha for the docker image",
	     DockerRepo: "url for the docker registry",
	     GitCommit: "git commit",
	     GitRepo: "git repo",
	     GitTag: "git tag",
	     GitUrl: "git url",
	     buildnumber: env.BUILD_ID, 
	     buildjob: "GLOBAL.test-project",
	     ComponentType: "Wildfly_EP",
	     ChangeRequestDS: "GLOBAL.JiraUnisys",
	     Category: "General",
	     AlwaysDeploy: "N",
	     DeploySequentially: "N",
	     BaseDirectory: "tmp",
	     PreAction: "GLOBAL.Run_SQL_Script_Postgres",
	     PostAction: "GLOBAL.Run_SQL_Script_Postgres",
	     CustomAction: "GLOBAL.Run_SQL_Script_Postgres",
	     Summary: "Pipeline Comp"
	    ];
	    
  echo "${attrs}";
  data = dh.updateComponentAttrs(url, user, pw, comp, "", version , attrs);
  echo "Update Done " + data.toString();
	    
  data = dh.newApplication(url, user, pw,application,appver, envs);
  appid = data[0];
	    
  if (appid > 0 && compid > 0)
  {
   def parent_compid = 0;
   def xpos = 100;
   def ypos = 100;
	  
   data = dh.assignComp2App(url, user, pw, appid, compid, parent_compid, xpos, ypos);
   echo data.toString();
  }
 }
	    
}
