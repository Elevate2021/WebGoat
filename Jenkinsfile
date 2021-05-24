#!/usr/bin/env groovy
pipeline {
  agent none
  options { 
    buildDiscarder(logRotator(numToKeepStr: '5'))
    skipDefaultCheckout true
  }
  stages {
    stage('Maven Build ') {
      when { 
       not {
           branch 'release-*'
       }
        beforeAgent true
      }
      agent {
        kubernetes{
          label 'maven'
          yamlFile 'kubernetes-agents/mvn.yaml'
        }
      }
    steps {
     container('maven') {
        //sleep time: 10, unit: 'MINUTES'
        checkout scm
        sh '''
        mvn clean install -s /usr/share/maven/ref/settings.xml
        '''
        stash includes: 'webgoat-server/target/*SNAPSHOT.jar', name: 'webgoat-server-jars'

      }
        junit '**/target/surefire-reports/TEST-*.xml'

      }
    }
}
}