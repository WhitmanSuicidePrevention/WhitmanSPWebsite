---
layout:     post
title:      "Setting up AWS for PyTorch"
subtitle:   "Includes TorchVision and PySyft"
date:       2019-09-01 10:17:00
author:     "A. Leah Zulas"
header-img: "/images/posts/pic04.jpg"
tags: [ AWS, PyTorch, Machine Learning, PySyft, TorchVision]
comments: true
---

## How to set up your AWS Instance

Posted on September 1, 2019

<br>
This post is mostly here to remind me how I did this in the future, but it can also be useful for others. Just a heads up, I use older versions of PyTorch and Torch Vision to make sure that PySyft works. 
<br>
PySyft is a library designed to help with federated learning. It allows tensors to be turned into binaries so that they can more easily be sent as packets across ports or https. 
<br>
First, get yourself an account at https://aws.amazon.com
You will need to give them a credit card, as a free instance will not have enough memory to do the job and you will get memory failures. 
<br>
Next, in the AWS panel, select "Launch Instance" to begin setup for your new AWS instance.
<br>
![](https://alzulas.github.com/Website/LaunchInstance.png){: .center-image}
<br>
Then, select the type of OS you would like to run from. Preferences given to Ubuntu instances and "Linux" instances which are normally just CentOS. 
<br>
![](https://alzulas.github.com/Website/SelectMachine.png){: .center-image}
<br>
The next question will ask you what size machine you want. The more processors and RAM you select, the more expensive the instance. Unfortunately for this project, you cannot choose the free instance model. Running the server code alone will overwhelm the memory on the machine. I recommend large, just due to the amount of data that needs to be processed for machine learning. The task on the client side will run much faster if you choose this option. Medium will also work. 
<br>
![](https://alzulas.github.com/Website/SelectSize.png){: .center-image}
<br>
If you just choose select and launch, it will take you to the final screen where you can see the type of AWS server you have set up. 
<br>
![](https://alzulas.github.com/Website/Launch.png){: .center-image}
<br>
When you select launch it will ask you for a key. Use the top pull down menu to have it give you a key, and download it to a safe place on your machine. Make sure that you will have access to that location. And make sure that if your machine appends .txt onto the end of the key, that you remove that from the name. 
<br>
![](https://alzulas.github.com/Website/CreateKey.png){: .center-image height="700px" width="700px"}
<br>
Now you are ready to open a terminal and ssh into your new instance. If for any reason you have issues with the ownership of your key, use chmod 400 (name of your pem) to give yourself permission. 
<br>
To enter your instance, if the instance is Ubuntu, type into the terminal:
```bash
ssh -i nameofyourkey.pem ubuntu@your.aws.ip.address
```
If the instance is CentOS or other Linux
```bash
ssh -i nameofyourkey.pem ec2-user@your.aws.ip.address
```
<br>
![](https://alzulas.github.com/Website/Login.png){: .center-image height="700px" width="700px"}
<br>
Now you have a new instance. QUICK! Update! As with all cloud machines, you should expect that it may not be entirely up to date. So don't forget to update whenever you start up something new. 
<br>
Ubuntu:
```bash
sudo apt update
sudo apt upgrade -y
```
CentOS:
```bash
sudo yum update
```
<br>
And then do a quick reboot and log back in. 
<br>
The next part is pulled mostly from https://github.com/NovaVic/PySyft/wiki but includes several additions for new aws machines. 
<br>
To begin, just as in the turotial, set up Miniconda:
<br>
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod 755 Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh press yes to various prompts
```
and do init of conda as shown in prompt. With that, conda is added to ~/.bashrc
To activate the base conda virtual env:
```bash
source ~/.bashrc
```
Install pytorch and cudatool and torchvision, however, you will need an older version of both to make this work
```bash
conda install pytorch=1.0 torchvision cudatoolkit=9.0 -c pytorch
```
Now you will need to download PySyft and build from source, but to do that, we will first need git.
On Ubuntu
```bash
sudo apt-get install git
```
On CentOS
```bash
sudo yum install git
```
Now you can pull a clone of the PySft github
```bash
git clone https://github.com/OpenMined/PySyft.git
```
Next you will need to go into the PySyft that you just downloaded and change the requirement.txt file slightly, so that it accepts the correct versions of PyTorch and TorchVision.
```bash
cd PySyft
nano requirements.txt
```
Change these two areas:
torch==1.0
torchvision==0.2.2
<br>
Now we can install PySyft properly onto our new machine, but first we will need to install a gcc compiler. 
On Ubuntu
```bash
sudo apt install gcc
```
On CentOS
```bash
sudo yum group install "Development Tools"
sudo yum install man-pages
```
And finally we can install PySyft!
```bash
python setup.py install
```
You may also wish to add a newer version of Numpy and Jupyter notebooks, depending on which client you run.
```bash
conda install numpy
conda install jupyter notebook
```

When I was working on this setup, I was also working with a group in a "Secure and Private AI" class on Udacity. We had some working code for creating a client and a server, sending models from the client to the server, and then having the server calculate the overall model using the models it aquired from the clients. This tutorial utilizes the MNIST data set. You can also download the code from this project onto the machine, to start off running. For that, cd.. until you are back into the home area of the file system. Then:
```bash
git clone https://github.com/jess-s/SPAIC-Scorchers.git
```
And now you're ready to start a server and a client!
<br>

### ENJOY!

