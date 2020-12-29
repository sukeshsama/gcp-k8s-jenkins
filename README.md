# gcp-k8s-jenkins

* Setup Google cloud free account  
* Connet to your account https://console.cloud.google.com/
* From menu Select K8's Enginer and choose all defaults to setup 1 master 2 node cluster  
  
* Once cluster is ready, from left menu, select Applications and go to google market and install Jenkins( choose new namespace and provide namespace as 'jenkins')  
* Once Jenkins is available, connect to cluster and switch to jenkins namespace using below command  
 $ kubectl config set-context --current --namespace=jenkins  
* Connect to jenkins pod using below commands  
 $ kubectl get pods  
 $ kubectl exec -it jenkins-jenkins-0 /bin/bash -n jenkins  
 Run below command to get initial admin password for Jenkins  
 $ jenkins@jenkins-jenkins-0:/$ cat /var/jenkins_home/secrets/initialAdminPassword  
 
 * This step is must to create Dynamic kubernetes agents from Jenkins master  
 $ kubectl create sa jenkins -n jenkins  
 $ kubectl create -f role-bindings.yml -n jenkins (role-bindings.yml is available in this repo)
 
 * Connect to Jenkins from browser  
 1. Go to https://console.cloud.google.com/ -> Applications and click on Jenkin HTTP address  
 2. use Initial Password retrieved from Jenkins pod  
 3. you can create your admin account, if this step is ignored, you need to connect to jenkins pod every time to retrieve password, unless you save it somewhere.  
 4. Go to Manage Jenkins and Install Kubernetes plugin from Manage plugins section  
 
 # Configure kubernetes cloud(https://your-jenkinsurl-or-ip/configureClouds/  

    Kubernetes URL: https://kubernetes.default  
    Kubernetes server certificate key:  
    Disable https certificate check: off  
    Kubernetes Namespace: jenkins  
    Credentials: - none  
    Jenkins URL	: http://jenkins-jenkins-ui:8080/  
    Jenkins tunnel	jenkins-jenkins-agents-connector:50000  
    Jenkins URL and Jenkins tunnel are basically service & port numbers created during the Jenkins setup.  
    you can check them by running below commands  
    $ kubectl get svc -n jenkins
    
  # Configure Pod and container template
    * Pod Templates:  
    Name: test (you can choose anyname)  
    Labels: test (you can choose anyname)  
    * Containers:  
    Name: jnlp (leave jnlp, dont ask me why :))  
    Docker image: sukeshrsama/jnlp-test-agent:latest (dummy agent, to test Dynamic test agent)  
    Working directory: /home/jenkins/agent  
    Service Account: jenkins  
    
