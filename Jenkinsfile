#!/usr/bin/env groovy

@Library('deployhub') _

	
environment {
    DEPLOYHUB_CREDS = credentials('deployhub-creds')
}

def app="ChiliUptimeApp"
def environment=""
def cmd=""
def url="http://192.168.3.118:8181"
def user=env.DEPLOYHUB_CREDS_USR
def pw=env.DEPLOYHUB_CREDS_PSW

def dh = new deployhub();

node {
    echo(env.getEnvironment().collect({environmentVariable ->  "${environmentVariable.key} = ${environmentVariable.value}"}).join("\n"))
    echo(System.getenv().collect({environmentVariable ->  "${environmentVariable.key} = ${environmentVariable.value}"}).join("\n"))

	
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
 
	    echo "User=" + user
	    echo "PW=" + pw
 def comp="GLOBAL.Test_Project.Test.testapp"
 def application="GLOBAL.Test_Project.Test.My Test App"	 
 def appver = "5.0"	    
 def version = "0.1.0-106"
 def imagename = "app-ui-helm"
 def String[] envs = [ "GLOBAL.Test_Project.Test.dev", "GLOBAL.Test_Project.Test.Test"]	    

 echo "${url}";
 echo "${version}";

 // create component version
 compid = dh.newComponentVersion(url, user, pw, comp, "", version);
 echo "Creation Done " + compid.toString();

 // update attrs
 def attrs = [
	     buildnumber: env.BUILD_ID,
	     buildjob: "GLOBAL.test-project",
	     ComponentType: "Helm Chart",
	     Category: "Deploy",
	     AlwaysDeploy: "Y",
	     DeploySequentially: "Y",
	     CustomAction: "GLOBAL.HelmUpgradeAction",
	     'creds["helmrepo"]': "ec2user",
	     Chart: "stable/heartbeat",
	     ChartVersion: "1.0.0",
	     ChartNamespace: "dev1",
	     'image.tag': "1.0.0"
	    ];
	    
  echo "${attrs}";
  data = dh.updateComponentAttrs(url, user, pw, comp, "", version , attrs);
  echo "Update Done " + data.toString();
	    

 }
	    
}
