# jenkins
Continuous integration

Continuous delivery

Continuous deployment

cicd flow with jenkins with s3

webhook

How will you secure Jenkins

jenkins master slave

Continuous integration
=======================
Continuous Integration(CI) is a continuous process of of merging new code into the centralized repository

Developers practicing continuous integration merge their changes back to the main branch as often 
as possible. The developer's changes are validated by creating a build and running
 automated tests against the build. By doing so, you avoid integration challenges 
 that can happen when waiting for release day to merge changes into the release branch.

Continuous integration puts a great emphasis on testing automation to check that the
 application is not broken whenever new commits are integrated into the main branch.

Continuous delivery
 ==============
Continuous delivery is an extension of continuous integration since it automatically deploys all code changes to a testing and/or production environment after the build stage. 

This means that on top of automated testing, you have an automated release process and you can deploy your application any time by clicking a button.

In theory, with continuous delivery, you can decide to release daily, weekly, fortnightly, or whatever suits your business requirements. However, if you truly want to get the benefits of continuous delivery, you should deploy to production as early as possible to make sure that you release small batches that are easy to troubleshoot in case of a problem.

Continuous deployment
=======================
Continuous deployment goes one step further than continuous delivery. With this practice, every change that passes all stages of your production pipeline is released to your customers. There's no human intervention, and only a failed test will prevent a new change to be deployed to production.

Continuous deployment is an excellent way to accelerate the feedback loop with your customers and take pressure off the team as there isn't a Release Day anymore. Developers can focus on building software, and they see their work go live minutes after they've finished working on it.



1)Developer builds the code pushes to scm
 protocal:http/ssh
 webhooks: git triggers the job , webhooks only transfer data when there is new data to send,
 pollscm:Poll SCM" polls the SCM periodically for checking if any changes/ new commits were made

2)scm sends events (webhooks) to jenkins server

3)jenkins job will start and takes from SCM and copies to workspace
  
  Build
  ------
    workspace: *.java----compile-----*.class----*.workspace
    maven: mvn clean package

    Junit Test cases---test their code themself
    code qulity testing:jacoc,cobertura,checkstyle

    Reports ----html/json----sonarqube
    
 After all goes in the build phase we will get a package/target/binary/artifact
 example: *.workspace

step4: Save the package/delivery into artifactory 
ex:s3, nexus,jfrog

step5: take the package from artifactory and copies in to workspace
step6: jenkins has package in workspace and copie to target servers
step7:test the functionality with test cases

warpath=/var/lib/jenkins/workspace/build-war-maven/target/
warfile=hello-world-war-1.0.0.war
bucketname=rctcloud
aws s3 cp $warpath/$warfile s3://$bucketname/



/opt/maven38/bin/mvn clean package 
sleep 5

warpath=/var/lib/jenkins/workspace/build-war-maven/target
warfile=hello-world-war-1.0.0.war
newwarfile=sparkhello-$BUILD_NUMBER
bucketname=rctcloud
mv $warpath/$warfile $warpath/$newwarfile
aws s3 cp $warpath/$warfile s3://$bucketname/


deploy the war from s3
-----------------------

Download the package from s3 to the workspace

aws s3 cp s3://rctcloud-artifactory/sparkhello-10.war .

copy to the tomcat /webapps

scp packagename.war  username@serverIP:/opt/tomcat9/webapps/
scp sparkhello-10.war root@192.168.4.148:/opt/tomcat9/webapps/sparkhello.war



pass parameter from one job to other job
pre-req: plugin

BUILD_NUMBER----CI



webhooks-
=========

A webhook is a mechanism to automatically trigger the build of a Jenkins project upon a commit pushed in a Git repository

Settings →> Integrations & Services →> Add service →> Search “Jenkins (Github Plugin)” →> Add name as anything and url as http://19.128.43.75:8081/github-webhook/


How will you secure Jenkins
====================

Make sure that global security is on.
Check if Jenkins is integrated with my company's user directory with an appropriate plugin.
Ensure that the matrix/Project matrix is enabled to fine-tune access.
Automate the process of setting rights/privileges in Jenkins with custom version controlled script.
Limit physical access to Jenkins data/folders.
Periodically run security audits on the same



jenkins master slave
==============
 Jenkins dashboard, 
 Manage jenkins 
 Manage Nodes 			
 New Node 

You have to give your slave name like “Jenkins_Slave”


COMMAND:   ssh hostname java -jar ~/bin/agent.jar.

Like 
ssh ec2-user@172.31.95.252 java –jar /home/ec2-user/agent.jar


 You have to copy link address from agent.jar can be downloaded from here.

To download agent.jar file in Jenkins slave. 

CMD:	

wget http://18.212.19.254:8080/jnlpJars/agent.jar

  Finally Mange Jenkins  Configuration System  Usage
