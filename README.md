Wanderlust Mega Project: DevSecOps CI/CD PipelineThis repository hosts the Wanderlust Mega Project, featuring a fully automated DevSecOps pipeline. The architecture focuses on "Shifting Left" by integrating security scanning (SAST, SCA, and FS Scan) directly into the deployment workflow.🚀 Architecture OverviewCI/CD Engine: JenkinsStatic Analysis (SAST): SonarQubeSoftware Composition Analysis (SCA): OWASP Dependency-CheckVulnerability Scanning: TrivyRuntime: Docker & Docker Compose🛠️ Infrastructure Setup1. AWS EC2 ProvisioningDeploy two instances with the following specs:Type: m7i-flex-largeInstance 1: Jenkins-Server (Ports: 22, 8080)Instance 2: SonarQube-Server (Ports: 22, 9000)2. Jenkins Installation (Ubuntu)Bashsudo apt update && sudo apt install fontconfig openjdk-21-jre -y
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update && sudo apt install jenkins -y
3. Security Tools SetupTrivy Installation:Bashsudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update && sudo apt-get install trivy -y
🏗️ Pipeline ConfigurationJenkins Plugins RequiredInstall these via Manage Jenkins > Plugins:NodeJS (For frontend builds)SonarQube Scanner (Static code analysis)OWASP Dependency-Check (Library vulnerability scanning)Docker & Pipeline Stage View (Visualization and deployment)Global Tool ConfigurationEnsure the following names are used in Jenkins:NodeJS: Name it nodeSonarQube Scanner: Name it SonarDependency-Check: Name it dc🔗 Pipeline StagesThe Jenkinsfile in this repo automates the following:StageDescriptionCheckoutPulls the latest code from GitHub.InstallRuns npm ci for clean, consistent dependencies.SASTPushes code to SonarQube for quality and security analysis.SCAScans libraries via OWASP for known CVEs.Trivy ScanPerforms a filesystem scan; fails build on CRITICAL vulnerabilities.DeployUses Docker Compose to bring up the application environment.
🛑 Error ReportingIf you encounter any build failures:Check the Console Output in Jenkins.If it is a Trivy error, it means your code contains critical security risks.Open an Issue on this GitHub repository with the error log for support.


      










