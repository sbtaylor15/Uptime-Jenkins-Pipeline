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
 

      def comp="GLOBAL.American University.CSC589.chili.Development.testcomp4"
      def application="GLOBAL.American University.CSC589.chili.Development.TestApp"	 
      def appver = "1"	    
      def version = "0.1.0-109"
      def imagename = "app-ui-helm"
      def String[] envs = ["GLOBAL.American University.CSC589.chili.Development.Dev"]	    

      echo "${url}";
      echo "${comp}";
      echo "${version}";
      def component_items = [
          [name:'f_index',repo: 'GLOBAL.American University.CSC589.chili.JenkinsRepo', uri: 'http://google.com5', pattern: 'index.html'],
          [name:'f_readme',repo: 'GLOBAL.American University.CSC589.chili.JenkinsRepo', uri: 'http://read.google.com5', pattern: 'read.me'],
          [name:'f_help',repo: 'GLOBAL.American University.CSC589.chili.JenkinsRepo', uri: 'http://help.google.com5', pattern: 'help.asp'],
      ];

 
      // create component version
      compid = dh.newComponentVersion(url, user, pw, comp, "", version, "file",component_items);
      echo "Creation Done " + compid.toString();

    def attrs = [
        buildnumber: env.BUILD_ID,
        //buildjob: "",
        ComponentType: "",
        Category: "Deploy",
        AlwaysDeploy: "Y",
        //DeploySequentially: "Y",
        PostAction: "",
        BaseDirectory: "opt"
      ];

     echo "${attrs}";
      data = dh.updateComponentAttrs(url, user, pw, comp, "", version , attrs);

      echo "Update Done " + data.toString();
  }
}
