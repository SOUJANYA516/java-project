def label = "mypod-${UUID.randomUUID().toString()}"
def serviceaccount = "jenkins-admin"
podTemplate(label: label, serviceAccount: serviceaccount, 
    containers: [containerTemplate(name: 'maven', image: 'localhost:32121/root/docker_registry/maven', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'curl', image: 'localhost:32121/root/docker_registry/aiindevops.azurecr.io/curl:0.1', ttyEnabled: true, alwaysPullImage: true, command: 'cat'),
        containerTemplate(name: 'git-secrets', image: 'localhost:32121/root/docker_registry/aiindevops.azurecr.io/git-secrets:0.1', ttyEnabled: true, alwaysPullImage: true, command: 'cat'),
        containerTemplate(name: 'go', image: 'localhost:32121/root/docker_registry/golang:alpine3.9', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'kubeaudit', image: 'localhost:32121/root/docker_registry/aiindevops.azurecr.io/kube-audit:0.1', ttyEnabled: true, alwaysPullImage: true, command: 'cat'),
        containerTemplate(name: 'helm', image: 'localhost:32121/root/docker_registry/lachlanevenson/k8s-helm:v2.9.1', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'kubectl', image: 'localhost:32121/root/docker_registry/aiindevops.azurecr.io/docker-kubectl:19.03-alpine', ttyEnabled: true, command: 'cat',
               volumes: [secretVolume(secretName: 'kube-config', mountPath: '/root/.kube')]),
    containerTemplate(name: 'docker', image: 'localhost:32121/root/docker_registry/docker:1.13', ttyEnabled: true, command: 'cat')],
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        imagePullSecrets: [ 'gcrcred' ]
    ) 


{
        node(label) {
               def GIT_URL = 'https://github.com/SOUJANYA516/java-project'
               def GIT_CREDENTIAL_ID = 'SOUJANYA516'
               def GIT_BRANCH = 'master'
        
               /***For ReactJs - COMPONENT_KEY value should be same as what is given as ProjectName in in sonar.properies file ***/             
                def COMPONENT_KEY = 'sweetshop-react';
               def rootDir = pwd()    
               def SONAR_UI = 'http://sonar.ethan.svc.cluster.local:9001/sonar/api/measures/component?metricKeys=';
               
               /**** provide Sonar metrickeys which needs to be published to Jenkins console ***/
        //      String metricKeys = "coverage,code_smells,bugs,vulnerabilities,sqale_index,tests,ncloc,quality_gate_details,duplicated_lines_density";
        String metricKeys = "major_violations,minor_violations,critical_violations,blocker_violations,security_rating,complexity,violations,open_issues,test_success_density,test_errors,test_execution_time,security_remediation_effort,uncovered_conditions,classes,functions,line_coverage,sqale_rating,sqale_debt_ratio,reliability_remediation_effort,coverage,code_smells,bugs,vulnerabilities,sqale_index,tests,ncloc,quality_gate_details,duplicated_lines,cognitive_complexity";
               /*** Below variables used in the sonar maven configuration ***/
               def SONAR_SCANNER = 'org.sonarsource.scanner.maven'
               def SONAR_PLUGIN = 'sonar-maven-plugin:3.2'
               def SONAR_HOST_URL = 'http://sonar.ethan.svc.cluster.local:9001/sonar'
               
               /***  GCR_HUB Details  ***/
               def GCR_HUB_ACCOUNT = 'localhost:32121'
               def GCR_HUB_ACCOUNT_NAME = 'root'
               def GCR_HUB_REPO_NAME='docker_registry'
               //def DOCKER_CREDENTIAL_ID = 'gcrcred'
               def DOCKER_IMAGE_NAME = 'react-sweetshop'
               def IMAGE_TAG = 'v0.9'
               
               /*** Kuberenetes  ***/
               def K8S_DEPLOYMENT_NAME = 'sweetshop'
               
               try {
                                      
               stage('Build Project') {
                       container('maven') {
                               function.buildMethod()
                       }
               }      
            
                            
        /*      stage('Code Analysis') {
                       withCredentials([usernamePassword(credentialsId: 'SONAR', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
                               withSonarQubeEnv('SonarQube') {
                                       function.sonarMethod()
                               }  
                       }    
               } */
               
               /* Below stage is to publish tools logs to Jenkins console */
        /*      stage('Code Publish') {
                       container ('curl') {
                               String[] metricKeyList = metricKeys.split(",");
                               for(String key : metricKeyList){
                                      String var1=SONAR_UI+key+'&componentKey='+COMPONENT_KEY;
                                      sh('curl -k -u'+'"'+var1+'"');  
                               } 
                       }     
               } */
               /* 
               stage ('Build Container Image'){
                       container('docker'){
                               sh ("docker build -t ${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${IMAGE_TAG} .")
                       }
               } */ 
               
            stage (' Build Container Image'){
                       container('docker'){
                               //sh 'docker run busybox nslookup google.com'
                       sh ("docker build -t ${GCR_HUB_ACCOUNT}/${GCR_HUB_ACCOUNT_NAME}/${GCR_HUB_REPO_NAME}/${DOCKER_IMAGE_NAME}:${IMAGE_TAG} --network=host .")
                               }
                       }              
               
                                    }
		}
}
