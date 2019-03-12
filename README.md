![GitHub top language](https://img.shields.io/github/languages/top/azagramac/sample-java-maven-jenkins-nexus-sonar.svg) ![GitHub last commit (branch)](https://img.shields.io/github/last-commit/azagramac/sample-java-maven-jenkins-nexus-sonar/master.svg)

# sample-java-maven-jenkins-nexus-sonar

This repository is for the
[Build a Java app with Maven](https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Java application which outputs the string
"Hello world!" and is accompanied by a couple of unit tests to check that the
main application works as expected. The results of these tests are saved to a
JUnit XML report.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `scripts` subdirectory
contains a shell script with commands that are executed when Jenkins processes
the "Deliver" stage of your Pipeline.

Plugins Jenkins requires:
- artifact-promotion
- Maven Artifact ChoiceListProvider (Nexus)
- Maven Release Plug-in Plug-in
- Nexus Artifact Uploader
- Nexus Platform Plugin
- Pipeline Utility Steps
- Pipeline: GitHub Groovy Libraries
- SonarQube Scanner for Jenkins

How install plugins:
- Open URL http://ip_server_jenkins:8080/pluginManager/available

Sample on Jenkins:
![alt text](https://github.com/AzagraMac/sample-java-maven-jenkins-nexus-sonar/blob/master/src/img_sample.png)

_____________________________________________________________________________________



