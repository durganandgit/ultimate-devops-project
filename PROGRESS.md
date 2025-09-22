# DevOps Project Progress Log

This document tracks the steps I followed while implementing the [Ultimate DevOps Project](https://github.com/iam-veeramalla/ultimate-devops-project-demo).

---

Step 1: Repository Setup  
Cloned original repo and removed history.  
Created my own repo: durganandgit/ultimate-devops-project.  
Pushed initial commit with clean history.  

git clone https://github.com/iam-veeramalla/ultimate-devops-project-demo.git  
cd ultimate-devops-project-demo  
git remote remove origin  
git remote add origin https://github.com/durganandgit/ultimate-devops-project.git  

# Reset history  
rm -rf .git  
git init  
git branch -M main  
git remote add origin https://github.com/durganandgit/ultimate-devops-project.git  
git add .  
git commit -m "Initial commit - starting my DevOps project"  
git push -u origin main --force  

---

Step 2: Launch and Connect to EC2  
Launched an Ubuntu 24.04 EC2 instance (m7i.flex.large) on AWS.  
Connected to the instance using .pem key after fixing permissions.  

chmod 400 devops-demo.pem  
ssh -i devops-demo.pem ubuntu@<public-ip>  

---

Step 3: Update System Packages  
Updated system packages to ensure the latest versions are installed.  

sudo apt-get update  

---

Step 4: Install Docker  
Installed Docker Engine, CLI, Buildx, and Compose plugin.  
Verified installation with the hello-world container.  
Fixed Docker permissions for the ubuntu user.  

sudo apt-get install ca-certificates curl  
sudo install -m 0755 -d /etc/apt/keyrings  
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc  
sudo chmod a+r /etc/apt/keyrings/docker.asc  

echo \  
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \  
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \  
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  

sudo apt-get update  
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin  

# Verify  
sudo systemctl status docker  
sudo docker run hello-world  

# Add user to docker group  
sudo usermod -aG docker ubuntu  
# Logout and log back in for changes to apply  

---

Step 5: Install Kubectl  
Installed Kubernetes CLI (kubectl) for managing Kubernetes clusters.  
Verified installation.  

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"  

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check  
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl  

# Verify  
kubectl version --client  

---

Step 6: Install Terraform  
Installed Terraform from the official HashiCorp repository.  
Verified installation.  

sudo apt-get update && sudo apt-get install -y gnupg software-properties-common wget  

wget -O- https://apt.releases.hashicorp.com/gpg | \  
gpg --dearmor | \  
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null  

gpg --no-default-keyring \  
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \  
--fingerprint  

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com \  
$(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \  
sudo tee /etc/apt/sources.list.d/hashicorp.list  

sudo apt update  
sudo apt-get install -y terraform  

# Verify  
terraform -version  

