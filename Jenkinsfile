#!/usr/bin/env groovy

@Library('deployhub') _


 def app="ChiliUptimeApp"
 def environment=""
 def cmd=""
 def url="http://192.168.3.118:8181"
 def user="admin"
 def pw="admin"
 def dh = new deployhub();

 node {
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
 

      def comp="GLOBAL.ABC.DEVOPS.My Pipeline.Development.testapp"
      def application="GLOBAL.ABC.DEVOPS.My Pipeline.Development.My Test App"	 
      def appver = "5.0.5"	    
      def version = "0.1.0-109"
      def imagename = "app-ui-helm"
      def String[] envs = ["GLOBAL.ABC.DEVOPS.My Pipeline.Development.dev"]	    

      echo "${url}";
      echo "${version}";

      // create component version
      compid = dh.newComponentVersion(url, user, pw, comp, "", version);
      echo "Creation Done " + compid.toString();

      // update attrs
      def attrs = [
	     buildnumber: env.BUILD_ID,
	     ComponentType: "Helm Chart",
	     Category: "Deploy",
	     AlwaysDeploy: "Y",
	     DeploySequentially: "Y",
	     'creds["helmrepo"]': "ec2user",
	     Chart: "stable/heartbeat",
	     ChartVersion: "1.0.0",
	     ChartNamespace: "dev1",
	     'image.tag': "1.0.0"
	    ];
	    
      echo "${attrs}";
      data = dh.updateComponentAttrs(url, user, pw, comp, "", version , attrs);
      echo "Update Done " + data.toString();

      data = dh.newApplication(url, user, pw,application,appver, envs);
      echo "New Application " + data.toString();
      appid = data[0];
	  
      data = dh.addCompVer2AppVer(url, user, pw, appid, compid); 
  }
}
