import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL


@NonCPS
def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
def mvnHome = tool 'M3'
pipeline {
  agent  { label 'master' }
  environment {
    AWS_REGION = 'us-east-1'
  }
  stages { 
    stage("Build") {
        steps {
            sh "'mvn' -Dmaven.test.failure.ignore clean package"
        }
    }              
    stage("Result") {
        steps {
          script {
            junit '**/target/surefire-reports/TEST-*.xml'
            archive 'target/*.jar'
            hygieiaDeployPublishStep applicationName: 'MyApp', artifactDirectory: '/target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: '1.0.0', buildStatus: 'InProgress', environmentName: 'QA'
          }       
        }
    }        
  }
  post {
    always {
        cleanWs()        
    }
  }
}  






