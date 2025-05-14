
# EC2 Configuration & .NET Deployment Guide

## 1. Create and Launch EC2 Instance
- Launch a new EC2 instance from AWS Console.
- During the setup, create and **download the key pair** in `.pem` format.

## 2. Connect to EC2 Instance
After the instance is running, click the **Connect** button in the EC2 dashboard.

### Two Options to Access the EC2 Instance:
#### a. EC2 Instance Connect
- Use the browser-based SSH client for direct access.

#### b. SSH Client (for remote access)
- Navigate to the folder containing your `.pem` file.
- Run the following command (from SSH Example in the AWS console):

```bash
ssh -i "rest-ec2-keypair-v2-mh-2.pem" ec2-user@ec2-xxx-xxx-xxx-xxx.ap-south-1.compute.amazonaws.com
```

## 3. Install .NET Runtime
```bash
wget https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/8.0.15/aspnetcore-runtime-8.0.15-linux-x64.tar.gz
```

## 4. Install .NET SDK
```bash
mkdir -p $HOME/dotnet
tar zxf aspnetcore-runtime-8.0.15-linux-x64.tar.gz -C $HOME/dotnet
export DOTNET_ROOT=$HOME/dotnet
export PATH=$PATH:$HOME/dotnet
```

## 5. Verify Installation
```bash
ls
# Output: aspnetcore-runtime-8.0.15-linux-x64.tar.gz  dotnet
```

## 6. Create Deployment Folder
```bash
mkdir hello-world-ec2-mh2
```

---

## 7. Deploy .NET Code to EC2

### a. Publish the .NET Project Locally
On your local machine:
```bash
dotnet publish -o publish --os linux
```

### b. Upload Files to EC2
Ensure the `.pem` file and `publish` folder are in the same local directory, then run:

```bash
scp -i .\rest-ec2-keypair-v2-mh-2.pem .\publish\* ec2-user@ec2-xxx-xxx-xxx-xxx.ap-south-1.compute.amazonaws.com:~/hello-world-ec2-mh2
```

> - `rest-ec2-keypair-v2-mh-2.pem`: your PEM file  
> - `publish\*`: local publish folder  
> - `~/hello-world-ec2-mh2`: EC2 publish folder path  

---

## 8. Run the Application on EC2

### a. Navigate to the Publish Folder
```bash
cd hello-world-ec2-mh2/
ls
```

### b. Install ICU Package
```bash
sudo yum install -y libicu
```

### c. Run the Application
```bash
dotnet rtryt.dll --urls "http://*:8100"
```

> Use a custom port like `8100` (avoid using 80 or 5000)

---

## 9. Configure EC2 Security Group

- Go to **EC2 > Instances > Select your instance**
- Click on **Security > Inbound Rules**
- Add a new rule:
  - **Type**: Custom TCP
  - **Port Range**: 8100
  - **Source**: 0.0.0.0/0 (or your IP for restricted access)
- Also ensure **HTTP (port 80)** is allowed

---

## 10. Access Your App

Visit in browser:

```
http://ec2-xxx-xxx-xxx-xxx.ap-south-1.compute.amazonaws.com:8100
```

Replace `xxx-xxx-xxx-xxx` with your EC2 instance's public IP or DNS.
