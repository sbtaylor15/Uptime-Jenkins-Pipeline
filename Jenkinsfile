#!/usr/bin/env groovy

@Library('deployhub') _

withCredentials([usernamePassword(credentialsId: 'deployhub-creds', passwordVariable: 'pw', usernameVariable: 'user')]) 
{  	
 def app="ChiliUptimeApp"
 def environment=""
 def cmd=""
 def url="http://voltron:7171"

 def dh = new deployhub();

 node {
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
 

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

      data = dh.newApplication(url, user, pw,application,appver, envs);
      echo "New Application " + data.toString();
      appid = data[0];

      if (appid > 0 && compid > 0)
      {
       def parent_compid = 0;
       def xpos = 100;
       def ypos = 100;
	  
      data = dh.assignComp2App(url, user, pw, appid, compid, parent_compid, xpos, ypos);
     }    
  }
 }    
}
