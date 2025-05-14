Ec2 Configration 


1. create launch instance in aws EC2
	a. download key pair in .pem formate 

2. After create the instance then go to the connect button , there have two option to access the ec2 instance 
	a. direct ec2 connect, this option direct configration ssh client 
	b. SSh client: need when you remotely access from outside 
		=> GO to the pem folder 
		=> Run this coomand(Show in SSH Example): ssh -i "rest-ec2-keypair-v2-mh-2.pem" ec2-user@ec2-......ap-south-1.compute.amazonaws.com
3. run dot net runtime : wget https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/8.0.15/aspnetcore-runtime-8.0.15-linux-x64.tar.gz
4. run dot net sdk:  mkdir -p $HOME/dotnet && tar zxf aspnetcore-runtime-8.0.15-linux-x64.tar.gz -C $HOME/dotnet
	     export DOTNET_ROOT=$HOME/dotnet
                     export PATH=$PATH:$HOME/dotnet
5. command : ls (show intaller file) (aspnetcore-runtime-8.0.15-linux-x64.tar.gz  dotnet)
6. Create folder which folder i deploy: mkdir hello-world-ec2-mh2

	Ok Now deploy the code and publish the Ec2
        1. publish the project : D:\OneDrive - intellier.com\project\test-deploy\rtryt>    dotnet publish -o publish --os linux
        2. publish in the Ec2. :: try to pem file and publish folder are same folder. 
			a. go to the pem folder 
			b. C:\aws-v2> scp -i .\rest-ec2-keypair-v2-mh-2.pem .\publish\* ec2-user@ec2-......ap-south-1.compute.amazonaws.com:~/hello-world-ec2-mh2
			((=> .\rest-ec2-keypair-v2-mh-2.pem= pem file location 
			((=> .\publish\*   == publish folder location 
			((=> ec2-user@ec2-......ap-south-1.compute.amazonaws.com:~/hello-world-ec2-mh2  == ec2 path which you find in connect-> ssh client button
			((=> ~/hello-world-ec2-mh2   == ec2 publish folder

	Again go to the ec2 terminal 
7. go to ec2 publish folder: cd hello-world-ec2-mh2/
8. check all deploy file are here or not: ls (show all file) 
9. Icu package :  sudo yum install -y libicu
10. run the dot net project : dotnet rtryt.dll --uls "http://*:8100"   ** try to ignore 80,5000 port 
11. go to instance and then go to the "security" to chage the "Inbound Rules".  create a new Inbounce rules add your Port . also ensure that "http port 80" are avalible other wise add this
12. copy the public ec2 url with port Like= ec2-65-2-75-19.ap-south-1.compute.amazonaws.com/8100
