# Using Jupyter Notebooks with Your VM
This document will show you how to launch a Jupyter notebook server
on your EC2 machine and access it from a browser on your
Mac.

--------
## 1. Create an ssh config file
This will make sure your attempts to connect EC2 via ssh are tunneled through bastion (a proxy) automatically.

- Open your ~/.ssh/config file on your Mac. If you don't have this file, create it.
- Enter the following in your ~/.ssh/config file:

```
Host 11.11.my.ec2.ip.address.222
    ProxyCommand myusername@aproxy_server -W %h:%p
```

This file will also impact the behavior of other ssh-based activities involving EC2, like using ```scp``` to transfer files.


## 2. Install/Upgrade Anaconda and Jupyter

- On your local machine, download the Anaconda 64-bit installer
for Linux (Python 3 recommended): https://www.continuum.io/downloads

- Transfer the install file to your EC2 machine:
```
scp ~/path_to_install_file/Anaconda3-4.4.0-Linux-x86_64.sh 11.11.my.ec2.ip.address.222:/home/my_user_name
```
You could also download the install file directly to your EC2 machine using the ```curl``` command (but don't forget ```-o```!).

- Install Anaconda on your EC2 machine:
```
bash Anaconda3-4.4.0-Linux-x86_64.sh
```

## 3. Change Jupyter's settings on your EC2 machine

- Generate a notebook configuration file by entering the following into
your Terminal (in a VM session):
```
jupyter notebook --generate-config
```
- Edit the notebook IP address in your newly created config file:
```
#This is the default value (make sure it's commented out!):
#c.NotebookApp.ip = 'localhost'
#This is the new value (don't comment this one out!):
c.NotebookApp.ip = '127.0.0.1'
```

## 4. Create a Jupter notebook password on your EC2 machine

- Enter the following into into a Terminal session on your EC2 machine. Jupyter will then prompt you to enter and verify a password.
```
jupyter notebook password
```

## 5. Try it out!

- To launch the notebook server from your EC2 machine, enter the following:

```
jupyter notebook --no-browser --port=8889
```

- To open an ssh tunnel on your local machine, enter:

```
ssh -N -L localhost:8888:localhost:8889 11.11.my.ec2.ip.address.222
```

- Access your Jupyter server by navigating to  __localhost:8888__ in your browser.

### NOTE: This also works with Tmux & mosh. Happy Notebooking!!
