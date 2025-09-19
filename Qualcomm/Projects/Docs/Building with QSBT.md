---
up: "[[Qualcomm MoC]]"
tags:
  - "#type/qcom"
related: 
status: todo
created-date: 2025-03-11
summary:
---

1. go/qsbt
2. Register build host
	1. ![[qcom-qsbt-register.png]]
```bash
docker login -u  <username> -p <encrypted password> https://docker-registry.qualcomm.com

# you need to generated encrypted password for your decrypted one at https://docker-registry.qualcomm.com

# login command
docker login -u rajeg -p f7XIolvkHqnzL/mA4efyVJw+f4ldaKBrm2RcsyKqM4CPvJqkedAVXPTGDu1QkmmN https://docker-registry.qualcomm.com

sudo usermod -aG docker ${USER}
sudo chmod 666 /var/run/docker.sock
```
3. Select Build
	1. Match the version of Vendor, Kernel and System version number from the build page.
	2. ![[qsbt-multiple-si.png]]
	3. Once the job is submitted you will get two email that job has started