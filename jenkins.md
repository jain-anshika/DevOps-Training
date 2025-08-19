
# Jenkins Training Notes 🚀

Jenkins is an **open-source automation server** used for **Continuous Integration (CI)** and **Continuous Deployment (CD)**.

---

## 🔹 Installation

### On Jenkins server:
```bash
yum install fontconfig java-17-openjdk -y
````

Enable SSH on Jenkins and connected machines:

```bash
vim /etc/ssh/sshd_config
vim /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
# set:
# PermitRootLogin yes
# PasswordAuthentication yes

systemctl enable sshd --now
```

---

## 🔹 Create a Pipeline Job

1. **New Item** → Enter project name (`techno-project`) → Select **Pipeline** → OK
2. Go to **Pipeline Configuration** → Add script:

```groovy
pipeline {
    agent any

    stages {
        stage('Welcome') {
            steps {
                echo 'Hello Welcome to Jenkins'
            }
        }
        stage('Directory') {
            steps {
                sh "mkdir -p dire1/jaipur"
            }
        }
        stage('Redirection') {
            steps {
                sh 'echo "Hello welcome to Jenkins" > dire1/jaipur/welcome.txt'
            }
        }
        stage('Backup') {
            steps {
                sh "tar -cvf /root/archive.tar /etc"
            }
        }
    }
}
```

Apply & Save → Build.

---

## 🔹 Jenkins + GitHub Integration

```bash
mkdir /project-x && cd /project-x
git init
vim index.html
git add .
git commit -m "this is 1st commit"
git remote add project-x https://github.com/username/repo.git
git push project-x master
```

* Generate **GitHub Token** (PAT) → Use in Jenkins credentials.
* Configure `git config --global credential.helper store`.
* Setup **GitHub Webhook** to trigger Jenkins build on commits.

---

## 🔹 Jenkins Project Setup with Agents

**Machines Required:**

* Jenkins Server
* Agent Server
* Docker Server

### Steps:

1. Install Docker on agent & docker servers:

   ```bash
   systemctl enable docker --now
   ```

2. Install EPEL repo & Ansible on agent:

   ```bash
   dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
   yum install ansible -y
   ```

3. Configure inventory file on agent:

   ```ini
   [prod]
   <docker-server-private-ip>
   ```

4. Test Ansible:

   ```bash
   ansible -m ping prod
   ```

---

## 🔹 Jenkins GUI → Configure Agent Node

* Manage Jenkins → Nodes → Add New Node
* Remote root dir: `/myproject`
* Launch via SSH → Add credentials (`root`, password)
* Host key verification: **Non-verifying strategy**

---

## 🔹 Django Project Pipeline Example

```groovy
pipeline {
    agent { label 'agent-jen' }

    stages {
        stage('git-checkout') {
            steps {
                git 'https://github.com/username/social_media.git'
            }
        }
        stage('move files to ansible') {
            steps {
                sh 'mv /myproject/workspace/django/docker.yaml /root/ansible'
            }
        }
        stage('Assign execute permission') {
            steps {
                sh 'chmod a+x ./*.sh'
            }
        }
        stage('Build') {
            steps {
                sh './build.sh'
            }
        }
        stage('Deploy') {
            steps {
                sh './ansible.sh'
            }
        }
    }
}
```

Trigger: **GitHub hook**
Discard old builds: `2`

---

# ✅ Key Notes

* Jenkins server manages pipeline orchestration.
* Agent server runs build & deploy tasks.
* Docker server hosts the containerized application.
* GitHub Webhook triggers Jenkins automatically.

---

