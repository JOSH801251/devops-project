# devops-project
This project demonstrates the implementation of a complete CI/CD pipeline using Jenkins, Maven, Ansible, Nexus, SonarQube, Prometheus, and Grafana. The setup integrates various tools and environments to build, test, and deploy applications efficiently and monitor the entire pipeline in real-time.

Jenkins CI/CD Pipeline Project Architecture (Java Web Application)

![image](https://github.com/user-attachments/assets/238d5628-7b9e-4f49-a59d-81c005fba4ec)


Jenkins Complete CI/CD Pipeline Environment Setup
Create a GitHub Repository Jenkins-CICD-Pipeline-Project and push the code in this branch(main) to your remote repository (your newly created repository).

Go to GitHub (github.com)
Login to your GitHub Account
Create a Repository called "Jenkins-CICD-Project"
Clone the Repository in the "Repository" directory/folder in your local
Download the code in in this repository "Main branch":  https://github.com/awanmbandi/eagles-batch-devops-projects.git

Unzip the code/zipped file
Copy and Paste everything from the zipped file into the repository you cloned in your local
Add the code to git, commit and push it to your upstream branch "main or master"
Confirm that the code exist on GitHub
Jenkins/Maven/Ansible

Create an Amazon Linux 2 VM instance and call it "jenkins-maven-ansible"
Instance type: t2.medium
Security Group (Open): 8080, 9100 and 22 to 0.0.0.0/0
Key pair: Select or create a new keypair
User data (Copy the following user data): https://github.com/awanmbandi/eagles-batch-devops-projects/blob/maven-nexus-sonarqube-jenkins-install/jenkins-install.sh
Launch Instance
SonarQube

Create an Create an Ubuntu 18.04 VM instance and call it "SonarQube"
Instance type: t2.medium
Security Group (Open): 9000, 9100 and 22 to 0.0.0.0/0
Key pair: Select or create a new keypair
User data (Copy the following user data): https://github.com/awanmbandi/eagles-batch-devops-projects/blob/maven-nexus-sonarqube-jenkins-install/sonarqube-install.sh
Launch Instance
Nexus

Create an Amazon Linux 2 VM instance and call it "Nexus"
Instance type: t2.medium
Security Group (Open): 8081, 9100 and 22 to 0.0.0.0/0
Key pair: Select or create a new keypair
User data (Copy the following user data): https://github.com/awanmbandi/eagles-batch-devops-projects/blob/maven-nexus-sonarqube-jenkins-install/nexus-install.sh
Launch Instance
EC2 (Dev/Stage/Prod)

Create 3 Amazon Linux 2 VM instance and call them (Names: Dev-Env, Stage-Env and Prod-Env)
Instance type: t2.micro
Security Group (Open): 8080, 9100 and 22 to 0.0.0.0/0
Key pair: Select or create a new keypair
Prometheus

Create an Ubuntu 20.04 VM instance and call it "Prometheus"
Instance type: t2.micro
Security Group (Open): 9090 and 22 to 0.0.0.0/0
Key pair: Select or create a new keypair
Launch Instance
Grafana

Create an Ubuntu 20.04 VM instance and call it "Grafana"
Instance type: t2.micro
Security Group (Open): 3000 and 22 to 0.0.0.0/0
Key pair: Select or create a new keypair
Launch Instance
Slack

Go to the bellow Workspace and create a Private Slack Channel and name it "yourfirstname-jenkins-cicd-pipeline-alerts"
Link: https://app.slack.com/client/T043JRQBB5L/C044F5PH3DE
Configure Promitheus

Login/SSH to your Prometheus Server
Clone the following repository: https://github.com/awanmbandi/eagles-batch-devops-projects.git
Change directory to "eagles-batch-devops-projects"
Swtitch to the "prometheus-and-grafana" git branch
Run: ./install-prometheus.sh
Confirm the status shows "Active (running)"
Exit
Configure Grafana

Login/SSH to your Grafana Server
Clone the following repository: https://github.com/awanmbandi/eagles-batch-devops-projects.git
Change directory to "eagles-batch-devops-projects"
Swtitch to the "prometheus-and-grafana" git branch
Run: ls or ll (to confirm you have the branch files)
Run: ./install-grafana.sh
Confirm the status shows "Active (running)"
Exit
Configure The "Node Exporter" accross the "Dev", "Stage" and "Prod" instances including your "Pipeline Infra"

Login/SSH into the "Dev-Env", "Stage-Env" and "Prod-Env" VM instance
Perform the following operations on all of them
Install git by running: sudo yum install git -y
Clone the following repository: https://github.com/awanmbandi/eagles-batch-devops-projects.git
Change directory to "eagles-batch-devops-projects"
Swtitch to the "prometheus-and-grafana" git branch
Run: ls or ll (to confirm you have the branch files)
Run: ./install-node-exporter.sh
Confirm the status shows "Active (running)"
Access the Node Exporters running on port "9100", open your browser and run the below
Dev-EnvPublicIPaddress:9100 (Confirm this page is accessible)
Stage-EnvPublicIPaddress:9100 (Confirm this page is accessible)
Prod-EnvPublicIPaddress:9100 (Confirm this page is accessible)
Exit
Configure The "Node Exporter" on the "Jenkins-Maven-Ansible", "Nexus" and "SonarQube" instances

Login/SSH into the "Jenkins-Maven-Ansible", "Nexus" and "SonarQube" VM instance
Perform the following operations on all of them
Install git by running: sudo yum install git -y (The SonarQube server already has git)
Clone the following repository: https://github.com/awanmbandi/eagles-batch-devops-projects.git
Change directory to "eagles-batch-devops-projects"
Swtitch to the "prometheus-and-grafana" git branch
Run: ls or ll (to confirm you have the branch files including "install-node-exporter.sh")
Run: ./install-node-exporter.sh
Make sure the status shows "Active (running)"
Access the Node Exporters running on port "9100", open your browser and run the below
Jenkins-Maven-AnsiblePublicIPaddress:9100 (Confirm the pages are accessible)
NexusPublicIPaddress:9100
SonarQubePublicIPaddress:9100
Exit
Update the Prometheus config file and include all the IP Addresses of the Pipeline Instances that are running the Node Exporter API. That'll include ("Dev", "Stage", "Prod", "Jenkins-Maven-Ansible", "Nexus" and "SonarQube")

SSH into the Prometheus instance either using your GitBash (Windows) or Terminal (macOS) or browser
Run the command: sudo vi /etc/prometheus/prometheus.yml
Navigate to "- targets: ['localhost:9090']" and add the "IPAddress:9100" for all the above Pipeline instances. Ecample "- targets: ['localhost:9090', 'DevIPAddress:9100', 'StageIPAddress:9100', 'ProdIPAddress:9100', 'Jenkins-Maven-AnsibleIPAddress:9100'] ETC..."
Save the Config File and Quit
Open a TAB on your choice browser
Copy the Prometheus PublicIP Addres and paste on the browser/tab with port 9100 e.g "PrometheusPublicIPAddres:9100"
Once you get to the Prometheus Dashboard Click on "Status" and Click on "Targets"
Confirm that Prometheus is able to reach everyone of your Nodes, do this by confirming the Status "UP" (green)
Done
Open a New Tab on your browser for Grafana also if you've not done so already.

Copy your Grafana Instance Public IP and put on the browser with port 3000 e.g "GrafanaPublic:3000"
Once the UI Opens pass the following username and password
Username: admin
Password: admin
New Username: admin
New Password: admin
Save and Continue
Once you get into Grafana, follow the below steps to Import a Dashboard into Grafana to visualize your Infrastructure/App Metrics
Click on "Configuration/Settings" on your left
Click on "Data Sources"
Click on "Add Data Source"
Select Prometheus
Underneath HTTP URL: http://PrometheusPublicOrPrivateIPaddress:9090
Click on "SAVE and TEST"
Navigate to "Create" on your left (the + sign)
Click on "Import"
Copy the following link: https://grafana.com/grafana/dashboards/1860
Paste the above link where you have "Import Via Grafana.com"
Click on Load (The one right beside the link you just pasted)
Scrol down to "Prometheus" and select the "Data Source" you defined ealier which is "Prometheus"
CLICK on "Import"
Refresh your Grafana Dashbaord
Click on the "Drop Down" for "Host" and select any of the "Instances(IP)"
Update Your Jenkins file with your Slack Channel Name

Go back to your local, open your "Jenkins-CICD-Project" repo/folder/directory on VSCODE
Open your "Jenkinsfile"
Update the slack channel name on line "97"
Change name from "jenkins-cicd-pipeline-alerts" to yours
Add the changes to git, commit and push to GitHub
Confirm the changes reflects on GitHub
Copy your Jenkins Public IP Address and paste on the browser = ExternalIP:8080

Login to your Jenkins instance using your Shell (GitBash or your Mac Terminal)
Copy the Path from the Jenkins UI to get the Administrator Password
Run: sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Copy the password and login to Jenkins
Plugins: Choose Install Suggested Plugings
Provide
Username: admin
Password: admin
Name and Email can also be admin. You can use admin all through as we
Continue and Start using Jenkins
Once on the Jenkins Dashboard

Click on "Manage Jenkins"
Click on "Plugin Manager"
Click "Available"
Search and Install the following Plugings "Install Without Restart"
SonnarQube Scanner
Maven Integration
Pipeline Maven Integration
Maven Release Plug-In
Slack Notification
Install all plugings without restart
Confirm and make test your installations/setups
