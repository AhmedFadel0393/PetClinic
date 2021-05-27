pipeline {
agent any
environment {
tomcat_url = '192.168.29.129:9090'
project_name = '/petclinic'
}
stages {
stage('SCM Checkout') {
steps {
echo '>>> Start getting SCM code'
git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic'
echo '>>> End getting SCM code'
}
}
stage('clean Code With Maven') {
steps {
echo '>>> Start Clean using Maven'
sh 'mvn -B -DskipTests clean'
echo '>>> end Build using Maven'
}
}
stage('test') {
steps {
echo '>>> Start testing using Maven'
sh "mvn test"
echo '>>> end testing using Maven'
}
}
stage('build jar file') {
steps {
echo '>>> Start testing using Maven'
sh "mvn package"
echo '>>> end testing using Maven'
}
}
stage('Build war file') {
steps {
echo '>>> Start building using Maven'
sh 'jar -cvf spring-petclinic-2.4.5.war *'
sh "mv *.war target/"
echo '>>> End building using Maven'
}
}
stage('Sanity Check') {
steps {
sh '''#!/bin/bash
url="${tomcat_url}${project_name}"
read -ra result <<< $(curl -Is --connect-timeout 5 "${url}" || echo "timeout 500")
status=${result[1]}
[ $status -eq 200 ] && echo " $url with status $status is >>> Running"
[ $status -ge 400 ] && echo "bounce at $url with status >>>>> $status"
'''
}
}
}
}
