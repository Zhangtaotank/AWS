## Deploy Steps for AWS Ubuntu 14.04 LTS EC2 Instance

### Login to AWS Instance:

`ssh -i <your AWS Pem key file> ubuntu@<aws ipv4 Public ip>`

### Permission for the keyfile:
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'amazonec2.pem' are too open.
It is recommended that your private key files are NOT accessible by others.
This private key will be ignored.
bad permissions: ignore key: amazonec2.pem
Permission denied (publickey).

- Linux/Unix: Your key file must not be publicly viewable for SSH to work. Use this command if needed: chmod 400 mykey.pem
- Windows: Keys must only be accessible to the user they're intended for and no other account, service, or group.
-- GUI:
  1. [File] Properties - Security - Advanced
  2. Remove all users, groups, and services, except for the key's user, under Permission Entries
  3. Set key's user to Full Control
- Run the `ssh -i <your AWS Pem key file> ubuntu@<ipv4 Public ip>` again



### Install Python / Git

```
sudo apt-get update
sudo apt-get upgrade

# Install GIT
sudo apt-get install git

# Install Anaconda (Miniconda)
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

bash Miniconda3-latest-Linux-x86_64.sh

# To update the path (Make sure you said Yes when it asked to update path in the Miniconda install steps)
source ~/.bashrc
```

### Download App Source Code:

```
git clone https://github.com/sampathweb/iris-api.git


cd iris-api-app
python env/create_env.py
source activate env/venv
python env/install_packages.py

python ml_src/build_model.py
python run.py (Confirm that App is running)


sudo apt-get install supervisor
sudo vi /etc/supervisor/conf.d/iris-api.conf
<press i insert mode>

[program:iris-api]
autorestart = true
command = /home/ubuntu/iris-api/env/venv/bin/python /home/ubuntu/iris-api/run.py --debug=False --port=80
numprocs = 1
startsecs = 10
stderr_logfile = /var/log/supervisor/iris-api.log
stdout_logfile = /var/log/supervisor/iris-api.log
environment = PYTHONPATH="/home/ubuntu/iris-api/env/bin/"

<escape :wq>

sudo supervisorctl reload

<Your APP is live now>
```

### Test the App

1. Open Browser:  `http://<AWS IP>` (App is Live!)

2. Test in Command Line (Optional):

```
curl -i http://<aws ip address>/api/iris/predict -X POST -d '{ "sepal_length": 2, "sepal_width": 5, "petal_length": 3, "petal_width": 4}'
```

Congratulations you have deployed your App
