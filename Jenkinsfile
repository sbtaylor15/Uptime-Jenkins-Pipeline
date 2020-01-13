#!/usr/bin/env groovy

@Library('deployhub') _


 def app="ChiliUptimeApp"
 def environment=""
 def cmd=""
 def url="http://192.168.3.116:8181"
 def user="admin"
 def pw="admin"
 def dh = new deployhub();

 node {
    stage('Clone sources') {
        git url: 'https://github.com/DeployHubProject/Uptime-Jenkins-Pipeline.git'
    }
    
    stage ('Testing') {
      def comp="GLOBAL.American University.CSC589.chili.Development.testcomp3"
      def application="GLOBAL.American University.CSC589.chili.Development.ChiliUptimeApp"	 
      def appver = "1"	    
      def version = "0.1.0-109"
      def imagename = "app-ui-helm"
      def String[] envs = ["GLOBAL.American University.CSC589.chili.Development.Dev"]	    

      def d = dh.getLogs(url, user, pw, "560");
      echo "Logs " + d.toString();
				     
      echo "${url}";
      echo "${version}";


  }
}
